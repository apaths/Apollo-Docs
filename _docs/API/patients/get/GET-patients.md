# ```GET/``` patients

## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Request parameters](#request-parameters)
- [Successful responses](#successful-responses)
- [Cilent error responses](#client-error-responses)
- [Server error responses](#server-error responses)


## Description

Read `patient` objects by using the `GET/` verb. You need to supply a proper
and valid [authentication](#authentication) token and provide valid request
parameters to successfully execute a search.

## Autheticiation

You must be an authenticated user to retreive `patients` See [Authentication](../../../authentication/README.md).
Use the [`users/token`](../../users/get/token.md) endpoint to receive your token.
Supply the toekn in Authorization Header as a Bearer Token.

## Request parameters

### Query fields

Pass parameters to the resource using the following query parameters. All fields
are optional in unless noted.

| Parameter            | Type        | Required | Description                                       |
|----------------------|-------------| :------: |---------------------------------------------------|
| fields               | String      |          | Default: 'all'<br><ul><li>'all' - returns all [available fields](../../README.md#fields).</li><li>comma-separated list of patient [fields](../../README.md#fields</li></ul>  |
| first_name           | String      |          | Match the first_name. Note: ignores case.         |
| last_name            | String      |          | Match the last_name. Note: not case sensititive   |
| limit                | Number      |          | Default: 100<br><ul><li>Only return the specified number of matching cases. Value must be between 1-100</li></ul> |
| mrn                  | String      |          | Match the mrn (medicalRecordNumber) exactly       |
| skip                 | Number      |          | Default: 0<br><ul><li>Skip the first `skip` items. Use with limit to page resutls. Value must be >0</li></ul>     |

### Example request
```**GET/** patients?first_name=Bob&fields=first_name,last_name```


## Successful Resposnes

#### `200 OK`

**Condition** \
User is authnticated, authorized, and the 'patient' objects were retreived successfully.

** Returns** \
The objects matching the query parameters as an array. Only the fields selected
with the `fields` parameter are returned.

**Example return from above example request**
``` Javascript
[
  { first_name: "Bob", last_name: "Brinker"},
  { first_name: "Bob", last_name: "Newhart"},
  { first_name: "Bob", last_name: "Starr"}
]
```

## Client Errors

#### `400 BAD REQUEST`

**Condition** \
Invalid input was supplied.

**Returns** \
Status code and list of invalid parameters and messages.

**Example request**
```**GET/** patients?skip=notanumber&limit=1000```

**Example return**
``` Javascript
[
  {
    location: "query",
    param: "skip",
    value: 'notanumber`',
    msg: "Must be a positive integer number."
  },
  {
    location: "query",
    param: "limit",
    value: 1000,
    msg: "Must be a postitive integer, less than 100."
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

