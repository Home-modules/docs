# Errors

A request can either fail because of a variety of reasons and the app must be able to handle errors for any request.

## Error types

The error object has two fields: `code` and `message` which are used to determine the cause of the error. The `code` field is the same as the HTTP status code of the request. The error object might also have some other fields depending on the error type.

### 401 TOKEN_INVALID

The auth token is not valid. The auth token must be provided for every request except `account.login`. When this error is encountered, the token must be dropped and the login form should be shown to the user.

### 400 INVALID_REQUEST

The HTTP method used or the path of the request does not conform to the ones shown in [the documentation](connecting.md).

### 400 INVALID_REQUEST_JSON

The request data is not valid JSON data.

### 400 INVALID_REQUEST_TYPE

The request type (the `type` field in the request data) does not exist or is not implemented.

### 400 MISSING_PARAMETER

A request parameter is required but was not provided (e.g. `account.login` without `password` field)

- **Field `missingParameters`:** An array of missing parameter names.
  
  Examples:
  - `password`
  - `room.id`

### 400 INVALID_PARAMETER

A request parameter is provided but is of the wrong type or value.

- **Field `paramName`:** The name of the parameter which was passed incorrectly

### 400 PARAMETER_OUT_OF_RANGE

One of these cases:

- A number is too big or too small
- A string is too short or too long
- An array has too few or too many elements

&nbsp;

- **Field `paramName`:** The name of the problematic parameter

### 500 INTERNAL_SERVER_ERROR

An internal server error occurred. This is most likely a bug and should be reported.

### 429 TOO_MANY_REQUESTS

More than 10 requests in the last second were made. Wait for 1 second and resend the request.

### 404 NOT_FOUND

The requested resource was not found. Depends on the request.

- **Field `object`:** The object that was not found. Value depends on the request and can be found in the documentation of the request that return this error.

### Other error types

Some request can return errors not listed above. They are documented in the description of the request.
