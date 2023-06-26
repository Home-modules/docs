# Connecting to the hub and sending requests

The hub uses the local network to connect with your device (phone or computer) and receive requests.

For now, there is no straightforward way to find the hub IP address.

## Sending requests

Requests are sent to the hub via HTTP requests on the port 703.

### Methods

The supported methods are GET and POST.

#### HTTP GET

```
<ip address>:<port>/request/<token>/<request data>
```

- Replace `<ip address>` with the IP address of the hub.
- Replace `<port>` with the port on which the API is hosted. By default, it is 80 for HTTP and 443 for HTTPS.
- Replace `<token>` with the [auth token](authorization.md#the-auth-token).

  The token is not required when the request is of type `account.login`. Instead you can use any non-empty string (e.g. `"null"`)
- Replace `<request data>` with the URL-encoded JSON of the request.

#### HTTP POST

The request is similar to the GET method, except that the request data moves to the request body and is not URL-encoded.

```
<ip address>:<port>/request/<token>

<request data>
```

##### Token in header

```
<ip address>:<port>/request
Token: <token>

<request data>
```

Token can be moved to an HTTP header. For `account.login` requests, the header can be absent.

Token in request path takes precedence over token in request header.

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
> The actual response returned by the hub isn't formatted, it is minified. The formatted version is shown here to make this page easier to read.

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
