# POST/ shellorders


## Overview

- [Description](#description)
- [Authentication](#authentication)
- [Body parameters](#body-parameters)
  - [Body object](#body-object)
  - [Insurance object](#insurance-object)
  - [Patient object](#patient-object)
  - [Physician object](#physician-object)
  - [Specimen object](#specimen-object)
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

Create a new `shellorder` object by using the `POST/` verb. You need to supply
an [authentication](#authentication) token and provide a valid request
[body](#body-parameters). The server will [respond](#success-responses)
according to the results of the request.



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
| client_app                   | String      |          | Default: none<br>Name of the HMR or client appliation generating the order<br>ex: "gMed", "Practice Fusion"  |
| client_req_id                | String      |          | Default: none<br>An ID the client HMR uses to track the order. |
| client_mrn                   | String      | Yes      | Default: none<br>An ID the client HMR provides for the patien's Medical Record Number (mrn) |
| client_name                  | String      | Yes      | Default: none<br>Name of the ordering client |
| client_npi                   | String      | Yes      | Default: none<br>10-digit NPI number of the ordering customer  |
| data                         | Object      |          | Default: none<br>A structured JSON object for any optional data the interface needs to provide to the case object. Use this for any information that doesnt fit into the standard fields. |
| demographics_id              | Number      |          | Default: none<br>An ID (foreign key) to a [Demographics](../../demographics/README.md)  |
| diagnosis_comments           | String      |          | Default: none<br>Any additional comments |
| diagnosis_history            | String      |          | Default: none<br>Physician supplied diagnosis history. Used to provide background to reading pathologist. |
| diagnosis_post_op            | String      |          | Default: none<br>Physician supplied diagnosis information created after the procedure.|
| diagnosis_pre_op             | String      |          | Default: none<br>Physician supplied diagnosis information created prior to the procedure being performed   |
| location_name                | String      | Yes      | Default: none<br>Name of the facility where the procedure was performed  |
| location_npi                 | String      | Yes      | Default: none<br>10-digit NPI number of the facility where the procedure was performed |
| lis_case_id                  | Number      |          | Default: none<br>`caseID` for the Metaclinic LIS case created from this Order.  |
| lis_case_number              | String      |          | Default: none<br>`caseNumber` for the Metaclinic LIS case created fro this Order.  |
| patient                      | Object      | Yes      | Default: none<br>A [patient object](#patient-object) describing the patient. Only 'patient.first_name' and 'patient.last_name' are required |
| physician_primary            | Object      | Yes      | Default: none<br>A [physician object](#physician-object) for the primary (ordering) physician  |
| physician_secondary          | Object      |          | Default: none<br>A [physician object](#physician-object) for the secondary (referring) physician  |
| procedure_date               | String      | Yes      | Default: none<br>An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| procedure_type               | String      |          | Default: none<br>Type of procedure. Typcally ("EGD", "Colon", "Double").  |
| screening                    | Boolean     |          | Default: false<br>Indicates screeing procedure or not.  |
| specimens                    | Array       | Yes (1+) | Default: none<br>An array of [specimen objects] submitted for pathology  |
| stat                         | Boolean     |          | Default: false<br>Indicates stat testing required  |


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


### Physisican object

Supply the phyisican object as the `primary_physician` or `secondary_physican`
body parameters.

| Parameter                   | Type        | Required | Description            |
|-----------------------------|-------------| :------: |------------------------|
| first_name                  | String      | Yes      | Default: none<br>Physician's first name |
| last_name                   | String      | Yes      | Default; None<br>Physician's last name |
| npi                         | String      | Yes      |  Default: none<br>10-digit NPI number of the physician |


### Specimen object

Supply the specimen object as the 'specimens' array in the request body.

| Parameter                    | Type        | Required | Description           |
|------------------------------|-------------| :------: |-----------------------|
| anatomical_direction         | String      |          | Default: none<br>Direction (ex: "Proximal", "Distal") |
| anatomical_distance          | String      |          | Default: none<br>Distance measurement if provided (typically a number) |
| anatomical_site              | String      | Yes      | Default: none<br>Location of the sample (ex: "Esophagus", "Bowel")
| data                         | Object      |          | Default: none<br>Any additional JSON data usefult to the app |
| description                  | String      |          | Default; none<br>Description of the specimen |
| number                       | String      |          | Default: none<br>Specimen number if indicated  |
| ruleouts                     | String      |          | Default: none<br>A comma-separated list of Rule outs R/O _____<br>ex: R/O H.Pylori, R/O Barretts"]
| type                         | String      |          | Default: none<br>Specimen type. Typically "Biopsy" etc. |



### Example body sumbission

``` Javascript
{
  clientApp: "Provation",
  client_req_id: "12345",
  client_mrn "MRN32334",
  client_name: "Atlanta Medical Group",
  client_npi: "1248842544",
  data: {
    some: "other",
    other: "data"
  },
  diagnosis_comments: "Patient has painful abdomen",
  diagnosis_history: "Patient history of diabetes and colitis",
  diagnosis_post_op: "Polyps appear to be benign",
  diagnosis_pre_op: "Blockage of anterior cecum",
  location_name: "Buckhead medical plaza",
  location_npi:  "4534543453",
  patient: {
    date_of_birth: "1972-04-17",
    first_name: "Robert",
    last_name: "Johansen",
  },
  physician_primary: {
    first_name: "Louis",
    last_name: "Shirley",
    npi: "1235493233"
  },
  physician_secondary: {
    first_name: "Jackie",
    last_name: "Ancelet",
    npi: "1235495897"
  },
  procedure_date: "2019-05-12T17:34:23.000Z",
  procedure_type: "Upper GI Endoscopy",
  specimens: [
    { anatomical_direction: "Proximal",
      anatomical_distance: "33mm",
      anatomical_site: "J-pouch",
      data: {},
      description: "Polyop",
      number: "A",
      ruleouts: ["Crohns Disease", "Colitis"]
    }, {
      anatomical_direction: "Proximal",
      anatomical_distance: "33mm",
      anatomical_site: "Anus",
      data: {},
      description: "Polyop",
      number: "B",
      ruleouts: ["Crohns Disease", "Colitis"]
    }
  ],
}
```

## Success Responses

#### `201 CREATED`

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
