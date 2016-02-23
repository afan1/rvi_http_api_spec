# RVI HTTP API Spec (2016-2-23)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Schema](#schema)
- [Authentication](#authentication)
- [Response Codes](#response-codes)
  - [`HTTP 200` OK](#http-200-ok)
  - [`HTTP 400` Bad Request](#http-400-bad-request)
  - [`HTTP 401` Unauthorized](#http-401-unauthorized)
  - [`HTTP 403` Forbidden](#http-403-forbidden)
  - [`HTTP 404` Not Found](#http-404-not-found)
  - [`HTTP 409` Conflict](#http-409-conflict)
  - [`HTTP 429` Too Many Requests](#http-429-too-many-requests)
  - [`HTTP 430` Limit Exceeded](#http-430-limit-exceeded)
  - [`HTTP 500` Internal Server Error](#http-500-internal-server-error)
  - [`HTTP 501` Not Implemented](#http-501-not-implemented)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Schema

The API schema is written in [Swagger v2.0](http://swagger.io/specification). To
view the schema you can visit [this URL](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/smartcar/rvi_http_api_spec/master/swagger.yaml#/default)
or view it locally by following [these instructions](https://github.com/swagger-api/swagger-ui).

## Authentication

Authentication is outside the scope of the API specification and is up to the
implementation to decide on what authentication to use.

## Response Codes

The RVI HTTP API uses the following HTTP status codes to communicate the various
success and error states of requests.

Response types are defined by standard [HTTP Status Code Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Each non-200 status code returns the following format:

```json
{
  "error": "...",
  "message": "..."
}
```

+ `error`: the error name which corresponds to the error code it was returned
with. (e.g. A `validation_error` corresponds with a HTTP 400)
+ `message`: a more detailed description of the error

### `HTTP 200` OK

The request was successful, the corresponding resource will be returned.

### `HTTP 400` Bad Request

The request was malformed due to either bad syntax, or missing parameters

```json
{
  "error": "validation_error",
  "message": "Invalid or missing request parameters."
}
```

### `HTTP 401` Unauthorized

The provided credentials were incorrect.

```json
{
  "error": "authentication_error",
  "message": "Invalid or expired token provided."
}
```

### `HTTP 403` Forbidden

The credentials provided have insufficient permissions to access the
requested resource.

```json
{
  "error": "permission_error",
  "message": "Insufficient permissions to access requested resource."
}
```

### `HTTP 404` Not Found

The requested resource does not exist.

```json
{
  "error": "resource_not_found_error",
  "message": "The requested resource was not found."
}
```

### `HTTP 409` Conflict

The vehicle is not capable of performing the request in its current state. For
example, you cannot open the vehicle's trunk when the vehicle is moving.

```json
{
  "error": "vehicle_state_error",
  "message": "Vehicle is not capable of performing request in current state."
}
```

### `HTTP 429` Too Many Requests

The application has sent too many requests and must slow down.

```json
{
  "error": "rate_limiting_error",
  "message": "The application has sent too many requests and cannot be served due to the application's rate limit being exhausted."
}
```

### `HTTP 430` Limit Exceeded

The application has sent exceeded its monthly request limit.

```json
{
  "error": "monthly_limit_exceeded",
  "message": "The application has exceeded its monthly limit. Please upgrade the billing plan on the account dashboard."
}
```

### `HTTP 500` Internal Server Error

The server unexpectedly had an internal error. This may include failures with
vehicle to cloud communications.

```json
{
  "error": "server_error",
  "message": "Unexpected server error, please try again."
}
```


### `HTTP 501` Not Implemented

The vehicle is not capable of performing the request. For example, a vehicle
without a sunroof cannot perform an 'open sunroof' command.

```json
{
  "error": "not_capable_error",
  "message": "Vehicle is not capable of performing request"
}
```
