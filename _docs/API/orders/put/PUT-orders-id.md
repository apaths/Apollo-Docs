# PUT/ orders/{id}


## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Request construction](#request-construction)
  - [Path parameters](#path-parameters)
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

Allows user to update an `order` object by using the `POST/` verb. You need to supply an
[authentication](#authentication) token and provide a valid request
[body](#body-parameters). The server will [respond](#success-responses) according to
the results of the request.

## Authentication

You must be an authenticated user to create an order. See [Authentication](../../../authentication/README.md).
Use the [`users/token`](../../users/get/token.md) endpoint to receive your token.
Supply the token in the Authorization Header as a Bearer Token.


## Request construction

### Path parameters

Pass the objectID of the object you want to modify as a path parameter.

| Parameter                  | Type        | Required | Description                        |
|----------------------------|-------------| :------: |------------------------------------|
| id                         | String      | Yes      | Default: none<br>ID of the `order` to modify  |


### Body object

The Object's properties will be set to the value(s) provided in the body of the request
if all values are valid. These are the updatable fields

| Parameter                  | Type        | Description                        |
|----------------------------|-------------|------------------------------------|
| client_app                 | String      | Name of the HMR or client appliation generating the order<br>ex: "gMed", "Practice Fusion"  |
| client_req_id              | String      | An ID the submitting (HMR, typically) client uses to track the order. |
| client_mrn                 | String      | An ID the client HMR provides for the patienst medical record number |
| client_name                | String      | Default: none<br>Name of the ordering client |
| client_npi                 | String      | Default: none<br>10-digit NPI number of the ordering customer  |
| data                       | Object      | A structured JSON object for any optional data the interface needs to provide to the case object. Use this for any information that doesnt fit into the standard fields. |
| demog_id                   | Number      | ID of the demographic linked to this order ("shell" cases only) |
| diagnosis_comments         | String      | Any additional comments |
| diagnosis_history          | String      | Physician supplied diagnosis history. Used to provide background to reading pathologist. |
| diagnosis_post_op          | String      | Physician supplied diagnosis information created after the procedure.|
| diagnosis_pre_op           | String      | Physician supplied diagnosis information created prior to the procedure being performed   |
| is_shell_order             | Boolean     | Indicates if this is a "shell" order. |
| instructions               | String      | Physician instructions sent to the lab |
| insurance_primary          | Object      | An [insurance object](#insurance-object) for the primary insurance payer  |
| insurance_secondary        | Object      | An [insurance object](#insurance-object) for the secondary insurance payer  |
| insurance_tertiary         | Object      | An [insurance object](#insurance-object) for the primary insurance payer  |
| lis_case_id                | Number      | `caseID` of the Metaclinic LIS case created from this `Order`.  |
| lis_case_number            | String      | `caseNumber` of the Metalclnic LIS case created fro this `Order`. |
| location_name              | String      | Name of the facility where the procedure was performed  |
| location_npi               | String      | 10-digit NPI number of the facility where the procedure was performed |
| patient                    | Object      | A [patient object](#patient-object) describing the patient |
| physician_primary          | Object      | A [physician object](#physician-object) for the primary (ordering) physician  |
| physician_secondary        | Object      | A [physician object](#physician-object) for the secondary (referring) physician  |
| procedure_date             | String      | An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| procedure_type             | String      | Type of procedure. Typcally ("EGD", "Colon", "Double").
| specimens                  | Array       | Yes (1+) | Default: none<br>An array of [specimen objects] submitted for pathology  |
| screening                  | Boolean     | true indicates procedure is a screening  |
| stat                       | Boolean     | true to indicate if procedures requires stat testing |
| status                     | String      | One of: converted|deleted|pending

> NOTE: that for nested object (type is Array or Object) the entire object is replaced.

### Example body sumbission

``` Javascript
{
  client_app: "eCW",
  client_req_id: "12345",
  client_mrn "MRN32334",
  client_name: "Atlanta Medical Group, LLC",
}
```

## Success Responses

#### `200 OK`

**Condition** \
User is authenticated, authorized, and the `order` object was modified or added
successfully.

**Returns** \
The newly created object, with any fields added by the server.

**Example return**
``` Javascript
{
  /*...the new object... */
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
    param: "client_name",
    value: null,
    msg: "client_name is required"
  },
  {
    location: "body",
    param: "location_npi",
    value: "xyz",
    msg: "location_npi must be a 10-character string with only numbers and spaces"
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

