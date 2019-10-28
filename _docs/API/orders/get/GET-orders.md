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

Read `orders` objects by using the `GET/` verb. You need to supply an
[authentication](#authentication) token and provide a valid request
[body](#body-parameters). The server will [respond](#success-responses) according to
the results of the request.

## Authentication

You must be an authenticated user to retreive `orders`. See [Authentication](../../../authentication/README.md).
Use the [`users/token`](../../users/get/token.md) endpoint to receive your token.
Supply the token in the Authorization Header as a Bearer Token.

## Request construction

### Query fields

Pass parameters to the resource using the request body. All fields are
optional unless marked with `REQUIRED`.

| Parameter                  | Type        | Required | Description                        |
|----------------------------|-------------| :------: |------------------------------------|
| client_app                 | String      |          | Default: none<br>A comma separated list of EMR names<br>ex: "gMed", "Practice Fusion"  |
| client_name                | String      |          | Default: none<br>A comma separted list of client names. |
| client_npi                 | String      |          | Default: none<br>10-digit NPI number of the ordering customer  |
| client_mrn                 | String      |          | Default; none<br>Match the order's `client_mrn` value. You can submit more than one client_mrn separated by commas. |
| demog_id                   | Number      |          | Default: none<br>Matches the demographic id for the order (used only on "shell" orders)  |
| fields                     | String      |          | Default: 'all'<br>- 'all' to return all available fields<br>- A comma separated list of [Order fields](../README.md#fields)
| is_shell_order             | Boolean     |          | Default: none <br>Indicates if the order is a "shell" order, and  should anticipate a demog_id or not |
| lis_case_number            | String      |          | Defautl: none<br>The LIS case created from this order. |
| location_name              | String      |          | Default: none<br>A comman separated list of facility names where the procedure was performed  |
| location_npi               | String      |          | Default: none<br>A comma separated list of 10-digit NPI number of the facility where the procedure was performed |
| limit                      | number      |          | Default: 100<br>A number between 0-1000. Limit results to this many `orders`  |
| lis_case_number            | String      |          | Default: none<br>An LIS caseNumber to match.  |
| lis_case_id                | Number      |          | Default: none<br>An LIS caseID to match. |
| procedure_date_from        | String      |          | Default: none<br>An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| procedure_date_thru        | String      |          | Default: none<br>An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| skip                       | number      |          | Default: 0<br>A positive integer. Skips the result to the n-th supplied number |
| status                     | String      |          | Default: "All"<br>A comma separated list of [Order Status](../README.md#fields)
| imported_from              | String      |          | Default: none<br>An ISO8601 date/time stamp. Orders with `importDate` greater than or equal to this value.
| imported_thru              | String      |          | Default: none<br>An ISO8601 date/time stamp. Returns orders with `importDate` less than, or equal to this value.

### Example request

```GET /orders?clientApp=eCW&client_npi=43928872844&fields=client_app,client_name,client_record_id```


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
[
  {
    client_app: "eCW",
    client_name: "Atlanta Medical Center",
    client_record_id: "43534534",
  }, {
    client_app: "eCW",
    client_name: "Atlanta Medical Center",
    client_record_id: "908078098",
  }
  // ...
]
```


## Client error responses

#### `400 BAD REQUEST`

**Condition** \
Invalid input was supplied.

**Returns** \
Status code and list of invalid parameters and messages.

**Example request**
```GET /api/1/get/orders?skip=-1&client_npi=5```

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
