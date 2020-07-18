# PUT/ orders


## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Request construction](#request-construction)
  - [Body object](#body-object)
  - [Insurance object](#insurance-object)
  - [Patient object](#patient-object)
  - [Physician object](#physician-object)
  - [Specimen object](#specimen-object)
  - [Example body sumbission](#example-body-submission)
- [Success Responses](#success)
  - [`200 OK`](#201-created)
- [Client Error Responses](#client-errors-responses)
  - [`400 BAD REQUEST`](#400-bad-request)
  - [`401 UNAUTHORIZED`](#401-unauthorized)
  - [`403 FORBIDDEN`](#403-forbidden)
  - [`429 TOO MANY REQUESTS`](#429-too-many-requests)
- [Server Error Responses](#server-error-responses)
  - [`500 SERVER ERROR`](#500-server-error)
  - [`503 SERVICE UNAVAILABLE`](#503-service-unavailable)


## Description

Allows you to update a limited set of `order` fields by using the `POST/` verb.
You need to supply an [authentication](#authentication) token and provide a
valid request [body](#body-parameters). The server will [respond](#success-responses)
according to the results of the request.

## Authentication

You must be an authenticated user to create an order. See [Authentication](../../../authentication/README.md).
Use the [`users/token`](../../users/get/token.md) endpoint to receive your token.
Supply the token in the Authorization Header as a Bearer Token.


## Request construction

### Body object

The Object's properties will be set to the value(s) provided in the body of the request
if all values are valid. These are the prameters and updatable fields:

| Parameter                  | Type        | Description                        |
|----------------------------|-------------|------------------------------------|
| id                         | Array       | An array of IDs of the object(s) you wish to update. Each `id` must be a valid numeric value. You must send at least one id, but not more than 3000 |
| archived                   | Boolean     | Updates the archived field of the the objects in `id` list to the indicated value. |
| deleted                    | Boolean     | Updates the `deleted` field of the objects in `id` list to the indicated value. |
| status                     | String      | One of: converted|pending |

> NOTE: the set of updateable fields is very small. This prevents bulk errors from
  for impacting sensitive areas of the the `orders` field. These status fields are
  easily reversable if set incorrectly via an errant API call.

### Example body sumbission

``` Javascript
{
  id: [16654, 12234],
  archived: true
}
```

## Success Responses

#### `200 OK`

**Condition** \
User is authenticated, authorized, and the `order` object was modified or added
successfully.

**Returns** \
The an object that lists the ids of updated `orders` and the values updated.

**Example return**
``` Javascript
{
  updated: [16654, 12234],
  archived: true,
}
```

## Client Errors

#### `400 BAD REQUEST`

**Condition** \
Invalid input was supplied.

**Returns** \
Status code and list of invalid parameters and messages.

**Example return**
``` Javascript
[
  {
    location: "body",
    param: "id[12]",
    value: 'abc',
    msg: "id must be numeric"
  },
  {
    location: "body",
    param: "archived",
    value: "xyz",
    msg: "archived must be true or false"
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

