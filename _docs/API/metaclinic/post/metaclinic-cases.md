# `POST/` metaclinic/cases

# WIP:

## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Request](#request)
- [Success Responses](#success-responses)
- [Client Error Responses](#client-error-responses)


## Description

Pushes case into LIS


## Request

### Body fields

Pass parameters to the LIS using the request body.

| Parameter       | Type     | Description                                     |
|-----------------|----------|-------------------------------------------------|
| token           | String   | This is the **metaclinic** access token. Get the token by using the [`/metaclinic/token`](./metaclinic-token.md) endpoint   |
| caseObj         | Object   | This is the case object to be inserted. See Metaclinic's documentation on how to structure the document.  |


## Success responses

#### `200 OK`

**Condition** \
User is authenticated, authorized, and the `order` object was retreived
successfully.

**Returns** \
The objects matching the query parameters. Only fields selected with the `fields`
parameter are returned.

**Example return**
``` Javascript
{
  "operation": "create",
  "caseID": 256228,
  "caseNumber": "GI19-04998",
  "labID": 8,
  "labIdentifier": "APS_Lab",
  "message": "Case \"GI19-04998\" was created."
}
```

## Client error responses

#### `401 UNAUTHORIZED`
**Condition** \
User does not have the proper role for the requesed operation.

**Returns** \
A string with following message.

**Example return**
``` Javascript
"You must be properly authenticated."


