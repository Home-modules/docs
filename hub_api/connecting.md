# Connecting to the hub and sending requests

The hub uses the local network to connect with your device (phone or computer) and receive requests.

For now, there is no straightforward way to find the hub IP address.

## Sending requests

Requests are sent to the hub via HTTP requests on the port 703.

### Methods

The supported methods are GET and POST.

#### HTTP GET

```
<ip address>:703/<token>/<request data>
```

- Replace `<ip address>` with the IP address of the hub.
- Replace `<token>` with the auth token. The token is returned by the hub when you log in or change the username.

  The token is not required when the request is of type `account.login`. Instead you can use any non-empty string (e.g. `"null"`)
- Replace `<request data>` with the URL-encoded JSON of the request.

#### HTTP POST

The request is similar to the GET method, except that the request data moves to the request body and is not URL-encoded.

```
<ip address>:703/<token>

<request data>
```

### The auth token

The auth token is used as a proof of the user being logged in and also determines which account is performing the request.

Sending a `account.login` request (which doesn't require a token) or a `account.changePassword` request will return an auth token that must be used for future requests.

The token consists of two parts, separated by a colon (`:`):

- The username
- 32 bytes encoded in HEX

The hub stores the valid tokens for each users in-memory. Whenever a request is received, the hub checks the list of valid tokens for the user and rejects the request if the provided token is not in that list.  
All tokens will be invalidated upon server restart or after a week of disuse and thus need to login again.  
Each auth token can make no more than 10 requests per second, otherwise the `429 TOO_MANY_REQUESTS` error will be received.

### Responses

The response is in JSON format and contains the result of the request or the error, in case the request failed.

The response has a property `type` which indicates whether the request failed or succeeded.  
If the request succeeds, its value will be `"ok"`. And the result will be in the field `data`.  
If it fails, the value will be `"error"`. The error can be found in the `error` field.

```json
{
    "type": "ok",
    "data": {}
}
```

_^ A request succeeded without returning any data._

> **Note**  
> The response isn't formatted but is minified. The formatted version is shown here to make it easier to read.

```json
{
    "type": "ok",
    "data": {
        "token": "admin:vovgjo439j0g3j409tur409j4029tu094j094g0"
    }
}
```

_A login request succeeded and returned the auth token to be used for future requests._

```json
{
    "type": "error",
    "error": {
        "code": 401,
        "message": "LOGIN_PASSWORD_INCORRECT"
    }
}
```

_A login request failed because a wrong password was given._

**For a list of possible errors that can be returned, visit [errors](errors.md).**
