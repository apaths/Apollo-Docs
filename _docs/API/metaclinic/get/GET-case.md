# GET/ orders


## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Request construction](#request-construction)
  - [Query fields](#query-fields)
  - [Example request](#example-request)
- [Success responses](#success-responses)
  - [`200 OK`](#200-ok)
- [Client error responses](#client-error-responses)
  - [`400 BAD REQUEST`](#400-bad-request)
  - [`401 UNAUTHORIZED`](#401-unauthorized)
  - [`403 FORBIDDEN`](#403-forbidden)
  - [`429 TOO MANY REQUESTS`](#429-too-many-requests)
- [Server error responses](#server-error-responses)
  - [`500 SERVER ERROR`](#500-server-error)
  - [`503 SERVICE UNAVAILABLE`](#503-service-unavailable)


## Description

Fetch a metaclinic case objecy from the Metaclnic LIS (via proxy). You must
be authenticated and authorized on the Apollo app to use this route. The server
will [respond](#success-responses) according to the results of the request.

The `/metaclinic/case` route returns the entire case object. The returned fields
is not limited by a query term. If you only need to check if the case exists,
you can use the [`metaclinic/casecheck`](./GET-casecheck.md) endpoint.

## Authentication

You must be an authenticated user to retreive a `case`. See [Authentication](../../../authentication/README.md).
Use the [`users/token`](../../users/get/token.md) endpoint to receive your token.
Supply the token in the Authorization Header as a Bearer Token, or browser cookie..

## Request construction

### Query fields

Pass parameters as query parameters. All fields are
optional unless marked with `REQUIRED`.

| Parameter                  | Type        | Required | Description                        |
|----------------------------|-------------| :------: |------------------------------------|
| env                        | String      | REQUIRED | Default: none<br>Indicates what metaclinic envrionment to look in. Valid options are: `api`, `hotfix`,`staging-api` |
| lab                        | String      | REQUIRED | Default: none<br>Indicates what lab instance to be used. For example, use "APS_Lab" for the main APS instance. If you don't have a lean lab instance key, you can get it rom Metaclinic |
| caseNumber                 | String      | REQUIRED | Default: none<br>Supply a case number to be matched. ex: `AG20-000203` |

### Example request

```GET /metaclinic/case?env=hotfix&lab=APS_Lab&caseNumber=SH20-000014```


## Success responses

#### `200 OK`

**Condition** \
User is authenticated, authorized, and the `case` object was retreived
successfully.

**Returns** \
The objects matching the query parameters. The entire case object is returned.
See the Metaclinic docs for details of the object structure.

**Example return**
``` Javascript
{
  "caseID": 449930,
  "patient": {
      "firstName": "Wendy",
      "lastName": "Naylor",
      "middleName": null,
      "gender": "M",
      "dateOfBirth": "1999-12-12",
      "address": {
          "street1": "1502 Lima Drive",
          "street2": null,
          "city": "Jersey City",
          "state": "FL",
          "zip": "121212",
          "countryCode": "US"
      },
      "socialSecurityNumber": null,
      "medicalRecordNumber": "P100000000",
      "clinicalMedicalRecordNumber": "12345",
      "referringLabMedicalRecordNumber": null
  },
  ...

}
```


## Client error responses

#### `400 BAD REQUEST`

**Condition** \
Invalid input was supplied.

**Returns** \
Status code and list of invalid parameters and messages.

**Example request**
```GET /api/1/metaclinic/case?env=notavalidenv```

**Example return**
``` Javascript
[
  {
    location: "query",
    param: "env",
    value: "notavalidenv,
    msg: "Must include a valid envrionment name env|hotfix|stagin-api"
  },
  {
    location: "query",
    param: "lab",
    value: "",
    msg: "Must include a valid lab code"
  },
  {
    location: "query",
    param: "caseNumber",
    value: "",
    msg: "Must include a valid caseNumber"
  },
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


## Server error responses


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
