# GET/ orders/{id}


## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Request construction](#request-construction)
  - [Query fields](#query-fields)
  - [Example request](#example-request)
- [Success Responses](#success)
  - [`201 CREATED`](#201-created)
- [Client Error Responses](#client-errors-responses)
  - [`400 BAD REQUEST`](#400-bad-request)
  - [`401 UNAUTHORIZED`](#401-unauthorized)
  - [`403 FORBIDDEN`](#403-forbidden)
  - [`429 TOO MANY REQUESTS`](#429-too-many-requests)
- [Server Error Responses](#server-error-responses)
  - [`500 SERVER ERROR`](#500-server-error)
  - [`503 SERVICE UNAVAILABLE`](#503-service-unavailable)


## Description

Read an `order` object by using the `GET/` verb. You need to supply an an `orderID`
on the path and an [authentication](#authentication) token in the Authorization
Header as a Bearer Token. The server will [respond](#success-responses) according to
the results of the request.

## Authentication

You must be an authenticated user to retreive an `order`.
See [Authentication](../../../authentication/README.md). Use the
[`users/token`](../../users/get/token.md) endpoint to receive your token. Supply
the token in the Authorization Header as a Bearer Token.

## Request construction

### Path parameter

Pass the `orderID` of the desired `order` using the route path.

| Path Prarameter  | Type       | Requred | Description                         |
|------------------|------------| :-----: | ------------------------------------|
| id               | String     | Yes     | Default: none<br>`OrderID` for the desired `order` |

### Query fields

Pass parameters to the resource using the request body. All fields are
optional unless marked with `REQUIRED`.

| Parameter                  | Type        | Required | Description                        |
|----------------------------|-------------| :------: |------------------------------------|
| fields                     | String      |          | Default: none<br>- 'all' to return all available fields<br>- A comma separated list of [Order fields](../README.md#fields)

### Example request

```GET /orders/GVnfEj3KM3RVjLjKFqMqAib6vc6NZs4Z/?fields=client_app,client_name,client_record_id```


## Success Responses

#### `200 OK`

**Condition** \
User is authenticated, authorized, and the `order` object was retreived
successfully.

**Example Request** \
```GET /orders/GVnfEj3KM3RVjLjKFqMqAib6vc6NZs4Z?fields=client_app,client_name,client_req_id```

**Returns** \
The objects matching the query parameters. Only fields selected with the `fields`
parameter are returned.

**Example return**
``` Javascript
{
  id: "GVnfEj3KM3RVjLjKFqMqAib6vc6NZs4Z",
  client_app: "eCW",
  client_name: "Atlanta Medical Center",
  client_req_id: "43534534",
}
```


## Client Errors

#### `400 BAD REQUEST`

**Condition** \
Invalid input was supplied.

**Returns** \
Status code and list of invalid parameters and messages.

**Example request**
```https://   /api/1/get/orders/GVnfEj3KM3RVjLjKFqMqAib6vc6NZs4Z?skip=-1&client_npi=5```

**Example return**
``` Javascript
[
  {
    location: "query",
    param: "client_npi",
    value: 5,
    msg: "Must be a string of 10 numeric characters"
  },
  {
    location: "query",
    param: "skip",
    value: -1,
    msg: "Must be a positive integer"
  }
]
```

#### `401 UNAUTHORIZED`
**Condition** \
User does not have the proper role for the requesed operation.

**Returns** \
A string with following message.

**Example return**
``` Javascript
"You do not have the appropriate role"
```

#### `403 FORBIDDEN`
**Condition** \
User has not signed into the appliation. The token is either invalid
or missing.

**Returns** \
The following message as a string

**Example return**
``` Javascript
"You must sign in before you can do that."
```

#### `429 TOO MANY REQUESTS`
**Condition** \
User has supplied too many operations in too short of a time interval.

**Returns** \
The following message as a string

**Example return**
``` Javascript
"Too many requests. Take a break, man."
```


## Server Errors


#### `500 SERVER ERROR`
**Condition** \
This is a generic, temporary error. The server is having a problem
but should return to service soon. If this error persists--contact support.

**Returns** \
The following message as a string

**Example return**
``` Javascript
"Temporary server error. You may want to try again in a few minutes."
```

#### `503 SERVICE UNAVAILABLE`
**Condition** \
This is an intentional server shut down due to maintenance
(for example) but will return to service (it's not permanent). Unlike
`500 SERVER ERROR` there is no need to contact support if the error is encountered.
