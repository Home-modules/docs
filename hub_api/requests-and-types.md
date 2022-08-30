# Hub API requests and types

There are different requests and types in the hub API. They are defined in a TypeScript type definitions file found in [api.ts](https://github.com/Home-modules/hub/blob/master/src/api.ts).

## Reading the docs from the TypeScript type definitions

The file consists of several namespaces:

- ### `Request`
  
  Contains the types for requests. All requests and their parameters and errors are documented.

  There is also a type of the same name which is a union of all request types.

- ### `Response`

  Contains different request response types. One response type can be used by more than one request type.

  There is also a type of the same name which maps request types to their corresponding responses.

- ### `Error`

  Contains the errors that can be returned from a request. Each error type definition is an object consisting of the HTTP status, error message and any other properties.

  There is also a type of the same name which maps request types to the errors they can return.

  There is also one other type called `ResponseOrError` that contains the type of the response or error returned by a request.

- ### `T`

  Contains any other types, such as `Room`, `Device`, etc.
