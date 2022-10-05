# Authorization

## The auth token

All requests except `account.login` require an auth token. The auth token is used as a proof of the user being logged in and also determines which account is performing the request.

Sending a `account.login` request (which doesn't require a token) or a `account.changePassword` request will return an auth token that must be used for future requests.

The token consists of two parts, separated by a colon (`:`):

- The username
- 32 bytes encoded in HEX

The hub stores the valid tokens for each users in-memory. Whenever a request is received, the hub checks the list of valid tokens for the user and rejects the request if the provided token is not in that list.  
All tokens will be invalidated upon server restart or after a week of disuse and thus need to login again.  
Each auth token can make no more than 10 requests per second, otherwise the `429 TOO_MANY_REQUESTS` error will be received.

If a request fails with the 401 `TOKEN_INVALID` error, the auth token must be deleted and the login screen should be displayed.

## Logging in

Use the `account.login` request to log in and get a token. Since you don't have a token yet, you can use any non-empty string (e.g the previous token, `"null"`, etc.) and it will be valid.

A token will be returned in the response, which should be used for future requests.

**Example request:**

```json
{
    "type": "account.login",
    "username": "admin",
    "password": "admin",
    "device": "Android 10 on iPhone 15" // Don't try this at home
}
```

**Example response:**

```json
{
    "type": "ok",
    "data": {
        "token": "admin:vovgjo439j0g3j409tur409j4029tu094j094g0"
    }
}
```

## Logging out

Use the `account.logout` request to log out. The session will be terminated and the token will stop working. The app must delete the stored token and show the log in screen.

This request requires no parameters and nothing will be returned.

## Changing password

Use the `account.changePassword` request to change the password of the current account.

> **Note**  
> The current session must be at least 24 hours old, otherwise the `SESSION_TOO_NEW` error will be returned.

**Example request:**

```json
{
    "type": "account.changePassword",
    "oldPassword": "admin",
    "newPassword": "1234567" // Use a stronger password on the actual request. This is not my own password.
}
```

This request does not return anything.

### Changing username

Use the `account.changeUsername` request to change the current account's password. The new username should be 3 characters or longer, and must not be already in use.

> **Warning**  
> Since the auth token depends on the username, changing the username means that the token needs to be changed. Other sessions that are not currently active have no way to become aware of this event, so their tokens will be invalidated.
>
> TL;DR: This request terminates other sessions.

<!---->

> **Note**  
> The current session must be at least 24 hours old, otherwise the `SESSION_TOO_NEW` error will be returned.

**Example request:**

```json
{
    "type": "account.changeUsername",
    "username": "i-am-admin"
}
```

Returns a new token which should be used for future requests, similar to `account.login`.

To check whether a username is available for use and not taken, use the `account.checkUsernameAvailable` request.

**Example request:**

```json
{
    "type": "account.checkUsernameAvailable",
    "username": "i-am-admin"
}
```

**Example response:**

```json
{
    "type": "ok",
    "data": {
        "available": true
    }
}
```

## Active sessions

### Getting the list of active sessions

Use the `account.getSessionsCount` request to get the number of active sessions. The request doesn't have any parameters.

**Example response:**

```json
{
    "type": "ok",
    "data": {
        "sessions": 5
    }
}
```

To get the actual list with session details, use the `account.getSessions` request. This request does not have any parameters.

**Example response:**

```json
{
    "type": "ok",
    "data": {
        "sessions": [
            {
                "id": "b7415393c9c24fbfc7f3ef0ddea99377ae93c0be8e920aa5a0c8195174b06044",
                "device": "Android 10 on iPhone 15",
                "loginTime": 1585683000000,
                "lastUsedTime": 1585683000000,
                "ip": "127.0.0.1",
                "isCurrent": true
            },
            ...
        ]
    }
}
```

Each session has the following properties:

- `id`: A random HEX string that is used when terminating the session.
- `device`: Human readable information about the OS, browser and device used to log in. It is taken from the `device` parameter of `account.login`.
- `loginTime`: In unix time, the time when the session was created (i.e. the `account.login` request was made)
- `lastUsedTime`: In unix time, the last time the session was used to make a request.
- `ip`: The IP address from which the session was created.
- `isCurrent`: Whether the session is the one used to send this request.

### Terminating sessions

To terminate all other sessions, send a `account.logoutOtherSessions` request. This request does not have any parameters.

**Example response:**

```json
{
    "type": "ok",
    "data": {
        "sessions": 2 // 2 sessions were terminated
    }
}
```

To terminate a specific session, use the `account.logoutSession` request.

**Example request:**

```json
{
    "type": "account.logoutSession",
    "id": "b7415393c9c24fbfc7f3ef0ddea99377ae93c0be8e920aa5a0c8195174b06044" // Taken from the response of the `account.getSessions` request
}
```

This request does not return anything.
