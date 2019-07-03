# POST/ orders


## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Body parameters](#body-parameters)
  - [Insurance object](#insurance-object)
  - [Patient object](#patient-object)
  - [Physician object](#physician-object)
  - [Specimen object](#specimen-object)
  - [Body example](#body-example)
- [Success Responses](#success)
  - [`201 CREATED`](#201-created)
- [Client Error Responses](#client-errors-responses)
  - [`400 BAD REQUEST`](#400-bad-request)
  - [`401 UNAUTHORIZED`](#401-unauthorized)
  - [`403 FORBIDDEN`](#403-forbidden)
  - [`429 TOO MANY REQUESTS`](#429-too-many-requests)
- [Server Error Responses]
  - [`500 SERVER ERROR`](#500-server-error)
  - [`503 SERVICE UNAVAILABLE`](`503-service-unavailable)


## Description

Create a new `order` object by using the `POST/` verb. To successfully create
a new `order` you need to supply an [authorization](#authorization) token and
provide a valid [body](#body-parameters). After attempting the creation the
server will [respond](#responses) idicating the success for failure of the request.

## Authentication

You must be an authenticated user to create an order. See [Authorization](#authentication).
Use the [`users/token`](../../users/API/post/token.md) to received your token.
When calling the `POST/ orders` endpoint, supply the token in the Authorization
Header as a Bearer Token.

## Body parameters

Pass parameters to the resource using the request body. All fields below are
optional unless marked with `REQUIRED`.

| Parameter                  | Type        | Description                        |
|----------------------------|-------------|------------------------------------|
| clientApp                  | String      | **Required** Default: none<br>Name of the HMR or client appliation generating the order<br>ex: "gMed", "Practice Fusion"  |
| clientRecordNumberID       | String      | Default: none<br>An ID the client uses to track the order. |
| clientMedicalRecordNumber  | String      | Default: none<br>An ID the client HMR provides for the patienst medical record number |
| clientName                 | String      | **Required** Default: none<br>Name of the ordering client |
| clientNPI                  | String      | **Required** Default: none<br>10-digit NPI number of the ordering customer  |
| data                       | Object      | Default: none<br>A structured JSON object for any optional data the interface needs to provide to the case object. Use this for any information that doesnt fit into the standard fields. |
| diagnosisComments          | String      | Default: none<br>Any additional comments |
| diagnosisHistory           | String      | Default: none<br>Physician supplied diagnosis history. Used to provide background to reading pathologist. |
| diagnosisPostOp            | String      | Default: none<br>Physician supplied diagnosis information created after the procedure.|
| diagnosisPreOp             | String      | Default: none<br>Physician supplied diagnosis information created prior to the procedure being performed   |
| instructions               | String      | Default: none<br>Physician instructions sent to the lab |
| locationName               | String      | **Required** Default: none<br>Name of the facility where the procedure was performed  |
| locationNPI                | String      | **Required** Default: none<br>10-digit NPI number of the facility where the procedure was performed |
| patient                    | Object      | **Required** Default: none<br>A [patient object](#patient-object) describing the patient |
| primaryInsurance           | Object      | Default: none<br>An [insurance object](#insurance-object) for the primary insurance payer  |
| primaryPhysician           | Object      | **Required** Default: none<br>A [physician object](#physician-object) for the primary (ordering) physician  |
| procedureDate              | String      | **Required** Default: none<br>An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| procedureType              | String      | Default: none<br>Type of procedure. Typcally ("EGD", "Colon", "Double").
| secondaryInsurance         | Object      | Default: none<br>An [insurance object](#insurance-object) for the secondary insurance payer  |
| secondaryPhysician         | Object      | Default: none<br>A [physician object](#physician-object) for the secondary (referring) physician  |
| tertiaryInsurance          | Object      | Default: none<br>An [insurance object](#insurance-object) for the primary insurance payer  |
| specimens                  | Array       | **Require at least one** Default: none<br>An array of [specimen objects] submitted for pathology  |
|----------------------------|-------------|---------------------------------------------------------------------------------------|


### Insurance object

Supply the insurance object as a body parameter

| Parameter                   | Type        | Description                       |
|-----------------------------|-------------|-----------------------------------|
| groupNumber                 | String      | Default: none<br>Insurance group number            |
| insuredAddress              | String      | Default: none<br>Insured person's address          |
| insuredContact              | String      | **Required** Default: none<br>Insuran
| memberNumber                | String      | Default: none<br>Member's insurance number         |
| name                        | String      | **Required** Default: none<br>Name of the insurance payer  |
|-----------------------------|-------------|-----------------------------------|


### Patient object

Supply the patient object as a body parameter

| Parameter                   | Type        | Description                       |
|-----------------------------|-------------|-----------------------------------|
| addressStreet1              | String      | Default: none<br>Street address (line 1) |
| addressStreet2              | String      | Default: none<br>Street address (line 2) |
| city                        | String      | Default: none<br>City             |
| countryCode                 | String      | Dfeault: "US"<br>Any other of the ISO 2-digit country codes |
| dateOfBirth                 | String      | **Required** Default: none<br>Date of birth in "YYYY-MM-DD" format |
| email                       | String      | Default: none<br>Formatted email  |
| firstName                   | String      | **Required** Default: none<br>First name of the patient  |
| lastName                    | String      | **Requried** Default: none<br>Last name of the patient          |
| middleName                  | String      | Deafult: none<br>Middle name of the patient        |
| phoneNumber                 | String      | Default: none<br>Unformatted 10-digit phone number (ex 5043440342) |
| prefix                      | String      | Default: none<br>Name prefix (ex: "Mr.", "Ms.")    |
| sex                         | String      | **Required** Default: none<br>One of "M" or "F" |
| socialSecurityNumber        | String      | Default: none<br>Unformatted 9-digit number |
| state                       | String      | Default: none<br>State of patient |
| suffix                      | String      | Default: none<br>Name suffix (ex: "Jr.", "III")    |
| zip                         | String      | Default: none<br>ZIP code of patient |
|-----------------------------|-------------|------------------------------------|


### Physisican object

Supply the phyisican object as the `primaryPhysician` or `secondaryPhysican`
body parameters.

| Parameter                   | Type        | Description                       |
|-----------------------------|-------------|-----------------------------------|
| firstName                   | String      | Default: none<br>Physician's first name |
| lastName                    | String      | Default; None<br>Physician's last name |
| npi                         | String      | **Required** Default: none<br>10-digit NPI number of the physician |
|-----------------------------|-------------|-----------------------------------|


### Specimen object

Supply the specimen object as the 'specimens' array in the request body.

| Parameter                    | Type        | Description                      |
|------------------------------|-------------|----------------------------------|
| anatomicalDirection          | String      | Default: none<br>Direction (ex: "Proximal", "Distal") |
| anatomicalDistance           | String      | Default: none<br>Distance measurement if provided (typically a number) |
| anatomicalSite               | String      | Default: none<br>Location of the sample (ex: "Esophagus", "Bowel")
| data                         | Object      | Default: none<br>Any additional JSON data usefult to the app |
| description                  | String      | Default; none<br>Description of the specimen |
| number                       | String      | Default: none<br>Specimen number if indicated  |
| ruleOuts                     | Array       | Default: none<br>Any requested "rule outs", if supplied as an array.<br>ex: ["Rule Out Colitis"]
|------------------------------|-------------|----------------------------------|


### Body Example

``` Javascript
{
  clientApp: "eCW",
  clientRecordNumberID: "12345",
  clientMedicalRecordNumber "MRN32334",
  clientName: "Atlanta Medical Group",
  clientNPI: "1248842544",
  data: {
    some: "other",
    other: "data"
  },
  diagnosisComments: "Patient has painful abdomen",
  diagnosisHistory: "Patient history of diabetes and colitis",
  diagnosisPostOp: "Polyps appear to be benign",
  diagnosisPreOp: "Blockage of anterior cecum",
  instructions: "Please have pathologist read.",
  locationName: "Buckhead medical plaza",
  locationNPI:  "4534543453",
  patient: {
    addressStreet1: "237 N.Main Street",
    addressStreet2: "Apt. 6",
    city: "Jennings",
    countryCode: "US",
    dateOfBirth: "1972-04-17",
    email: "robert@goemail.com",
    firstName: "Robert",
    lastName: "Johansen",
    middleName: "M",
    phoneNumber: "3188245058",
    prefix: "Mr.",
    sex: "M",
    socialSecurityNumber: "114522444",
    state: "GA",
    suffix: "Jr",
    zip: "70456"
  },
  primaryInsurance: {
    groupNumber: "1534535345",
    insuredAddress: "1234 Oakdale Rd.",
    insuredContact: "Beth Orten",
    memberNumber: "34534534534",
    name: "Bluecross of Georgia",
  },
  primaryPhysician: {
    firstName: "Louis",
    lastName: "Shirley",
    npi: "1235493233"
  },
  procedureDate: "2019-05-12T17:34:23.000Z",
  procedureType: "EGD",
  specimens: [
    anatomicalDirection: "Proximal",
    anatomicalDistance: "33mm",
    anatomicalSite: "J-pouch",
    data: {},
    description: "Polyop",
    number: "A",
    ruleOuts: ["Crohns Disease", "Colitis"]
  ],
}
```

## Success Responses

### `201 CREATED`

**Condition** \
User is authenticated, authorized, and the `order` object was created
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

### `400 BAD REQUEST`

**Condition** \
Invalid input was supplied.

**Returns** \
Status code and list of invalid parameters and messages.

**Example return**
``` Javascript
[
  {
    location: "body",
    param: "clientName",
    value: null,
    msg: "clientName is required"
  },
  {
    location: "body",
    param: "locationNPI",
    value: "xyz",
    msg: "location must ba 10-character string with only numbers and spaces"
  }
]
```

### `401 UNAUTHORIZED`
**Condition** \
User does not have the proper role for the requesed operation.

**Returns** \
A string with following message.

**Example return**
``` Javascript
"You do not have the appropriate role"
```

### `403 FORBIDDEN`
**Condition** \
User has not signed into the appliation. The token is either invalid
or missing.

**Returns** \
The following message as a string

**Example return**
``` Javascript
"You must sign in before you can do that."
```

### `429 TOO MANY REQUESTS`
**Condition** \
User has supplied too many operations in too short of a time interval.

**Returns** \
The following message as a string

**Example return**
``` Javascript
"Too many requests. Take a break, man."
```


## Server Errors


### `500 SERVER ERROR`
**Condition** \
This is a generic, temporary error. The server is having a problem
but should return to service soon. If this error persists--contact support.

**Returns** \
The following message as a string

**Example return**
``` Javascript
"Temporary server error. You may want to try again in a few minutes."
```

### 503 SERVICE UNAVAILABLE
**Condition** \
This is an intentional server shut down due to maintenance
(for example) but will return to service (it's not permanent). Unlike
`500 SERVER ERROR` there is no need to contact support if the error is encountered.