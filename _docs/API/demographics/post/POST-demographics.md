# POST/ demographics


## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Body parameters](#body-parameters)
  - [Body object](#body-object)
  - [Insurance object](#insurance-object)
  - [Patient object](#patient-object)
  - [Example body sumbission](#example-body-submission)
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

Create a new `demographics` object by using the `POST/` verb. You need to supply an
[authentication](#authentication) token and provide a valid request
[body](#body-parameters). The server will [respond](#success-responses) according to
the results of the request.

## Authentication

You must be an authenticated user to create an order. See [Authentication](../../../authentication/README.md).
Use the [`users/token`](../../users/get/token.md) endpoint to receive your token.
Supply the token in the Authorization Header as a Bearer Token.

## Body parameters

### Body object

Pass parameters to the resource using the request body. All fields are
optional unless marked with `REQUIRED`.

| Parameter                    | Type        | Required | Description                        |
|------------------------------|-------------| :------: |------------------------------------|
| client_app                   | String      | Yes      | Default: none<br>Name of the HMR or client appliation generating the order<br>ex: "gMed", "Practice Fusion"  |
| client_mrn                   | String      |          | Default: none<br>An ID the client HMR provides for the patien's Medical Record Number (mrn) |
| client_name                  | String      | Yes      | Default: none<br>Name of the ordering client |
| client_npi                   | String      | Yes      | Default: none<br>10-digit NPI number of the ordering customer  |
| client_req_id                | String      |          | Default: none<br>An ID the client HMR uses to track the order. |
| data                         | Object      |          | Default: none<br>A structured JSON object for any optional data the interface needs to provide to the case object. Use this for any information that doesnt fit into the standard fields. |
| insurance_primary            | Object      |          | Default: none<br>An [insurance object](#insurance-object) for the primary insurance payer  |
| insurance_secondary          | Object      |          | Default: none<br>An [insurance object](#insurance-object) for the secondary insurance payer  |
| insurance_tertiary           | Object      |          | Default: none<br>An [insurance object](#insurance-object) for the primary insurance payer  |
| location_name                | String      | Yes      | Default: none<br>Name of the facility where the procedure was performed  |
| location_npi                 | String      | Yes      | Default: none<br>10-digit NPI number of the facility where the procedure was performed |
| patient                      | Object      | Yes      | Default: none<br>A [patient object](#patient-object) describing the patient |
| procedure_date               | String      | Yes      | Default: none<br>An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| status                       | String      |          | Default: `unmatched`<br>Status of the demographic record. `unmatched`|`matchedToOrder`|`deleted`  |

### Insurance object

Supply the insurance object as an `insurance_primary`, `insurance_secondary`, or
`insurance_tertiary` body parameter.

| Parameter                   | Type        | Required | Description            |
|-----------------------------|-------------| :------: |------------------------|
| group_number                | String      | Yes      | Default: none<br>Insurance group number  |
| insured_address             | String      |          | Default: none<br>Insured person's address  |
| insured_contact             | String      | Yes      | Default: none<br>Insuran  |
| insured_member_number       | String      | Yes      | Default: none<br>Member's insurance number  |
| insured_member_name         | String      | Yes      | Default: none<br>Name of the insured member (if different than PT)  |
| insured_relationship        | String      |          | Default: none<br>Rleationship between the patient and insured policy holder (ex: "Parent", "Spouse")  |
| member_number               | String      |          | Default: none<br>Patient's member number.  |
| name                        | String      | Yes      | Default: none<br>Name of the insurance payer  |


### Patient object

Supply the patient object as a body parameter

| Parameter                   | Type        | Required | Description            |
|-----------------------------|-------------| :------: |------------------------|
| address_street_1            | String      |          | Default: none<br>Street address (line 1) |
| address_street_2            | String      |          | Default: none<br>Street address (line 2) |
| city                        | String      |          | Default: none<br>City             |
| country_code                | String      |          | Default: "US"<br>Any other of the ISO 2-digit country codes |
| date_of_birth               | String      | Yes      | Default: none<br>Date of birth in "YYYY-MM-DD" format |
| email                       | String      |          | Default: none<br>Formatted email  |
| first_name                  | String      | Yes      | Default: none<br>First name of the patient  |
| last_name                   | String      | Yes      | Default: none<br>Last name of the patient  |
| middle_name                 | String      |          | Deafult: none<br>Middle name of the patient  |
| phone_number                | String      |          | Default: none<br>Unformatted 10-digit phone number (ex 5043440342) |
| prefix                      | String      |          | Default: none<br>Name prefix (ex: "Mr.", "Ms.")  |
| sex                         | String      | Yes      | Default: none<br>One of "M" or "F" |
| ssn                         | String      |          | Default: none<br>Unformatted 9-digit Social Security number |
| state                       | String      |          | Default: none<br>State of patient |
| suffix                      | String      |          | Default: none<br>Name suffix (ex: "Jr.", "III") |
| zip                         | String      |          | Default: none<br>ZIP code of patient |




### Example body sumbission

``` Javascript
{
  client_app: "eCW",
  client_mrn: "123456",
  client_name: "A Client Name",
  client_npi: "1234567890",
  client_req_id: "1234",
  insurance_primary: {
    group_number: "123-4567890",
    insured_member_name: "Rebecca Walker Secondary",
    insured_relationship: "self",
    name: "BCBS of Louisana",
    member_number: "123454679",
  },
  insurance_secondary: {
    group_number: "123-4567890",
    member_name: "Rebecca Walker Secondary",
    insured_relationship: "self",
    name: "BCBS of TX",
    member_number: "9876543219",
  },
  insurance_tertiary: {
    group_number: "987-654321",
    insured_member_name: "Rebecca Walker",
    insured_relationship: "self",
    name: "BCBS of Louisana",
    member_number: "123454679",
  },
  location_name: "Some Location Name",
  location_npi: "123457890",
  patient: {
    address_street_1: "Street line 1",
    address_street_2: "Street line 2",
    city: "Shreveport",
    state: "LA",
    country_code: "US",
    date_of_birth: "1977-02-27",
    email: "dandylion123@hotmail.com",
    first_name: "Rebecca",
    last_name: "Walker",
    middle_name: "Elizabeth",
    phone_number: "7135812234",
    prefix: "Ms.",
    sex: "F",
    ssn: "437396536",
    zip: "77003",
  },
  procedure_date: "2019-10-01T18:12:00.000Z",
  status: "unmatched",
}
```

## Success Responses

#### `201 CREATED`

**Condition** \
User is authenticated, authorized, and the `demographic` object was created
successfully.

**Returns** \
The newly created object, with any fields added by the server.

**Example return**
``` Javascript
{
  id: 124,
  client_app: "eCW",
  client_mrn: "123456",
  client_name: "A Client Name",
  client_npi: "1234567890",
  client_req_id: "1234",
  createdAt: "2019-01-01T07:00:44.000Z",
  insurance_primary: {
    group_number: "123-4567890",
    insured_member_name: "Rebecca Walker Secondary",
    insured_relationship: "self",
    name: "BCBS of Louisana",
    member_number: "123454679",
  },
  insurance_secondary: {
    group_number: "123-4567890",
    member_name: "Rebecca Walker Secondary",
    insured_relationship: "self",
    name: "BCBS of TX",
    member_number: "9876543219",
  },
  insurance_tertiary: {
    group_number: "987-654321",
    insured_member_name: "Rebecca Walker",
    insured_relationship: "self",
    name: "BCBS of Louisana",
    member_number: "123454679",
  },
  location_name: "Some Location Name",
  location_npi: "123457890",
  patient: {
    address_street_1: "Street line 1",
    address_street_2: "Street line 2",
    city: "Shreveport",
    state: "LA",
    country_code: "US",
    date_of_birth: "1977-02-27",
    email: "dandylion123@hotmail.com",
    first_name: "Rebecca",
    last_name: "Walker",
    middle_name: "Elizabeth",
    phone_number: "7135812234",
    prefix: "Ms.",
    sex: "F",
    ssn: "437396536",
    zip: "77003",
  },
  procedure_date: "2019-10-01T18:12:00.000Z",
  status: "unmatched",
  updatedAt: "2019-01-01T07:00:44.000Z",
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
