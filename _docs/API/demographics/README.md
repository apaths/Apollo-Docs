# Demographics

## Contents

- [Introduction](#introduction)
- [Fields](#Fields)
- [API reference](#api-reference)



## Introduction

the Demographics service provides endpoints for managing inbound request information
from EHRs or other locations. The data format is almost identical to the
[Orders](../orders/README.md) API, but with much less information.

To access the `demographics` resource:

- Register as user. See [Registration](../../registration/README.md)
- Hold and send an [Authentication token](../../authentication/README.md)
- Call [API endpoint](#api-reference) you desire



## Fields

The Demographcis service exposes these fields through the API. These fields are queryable
via the [GET/](./get/GET-demographics.md) route. Specify the desired fields in the `fields`
parameter.

| Field                         | Type        | Description                        |
|-------------------------------|-------------|------------------------------------|
| aps_case_body                 | Object.     | The actual case body sent to the Metaclinic API that was used to create the Metaclinic case in the APS instance. |
| aps_case_id                   | Number      | A number that identifies the Metaclinic case that demographics were pushed into APS instance. |
| aps_case_number               | String      | The case number of the Metaclinic case that received the demographics in the APS instance. |
| archived                      | Boolean     | Indicates if a demographic record has been archived. |
| client_app                    | String      | Name of the HMR or client appliation generating the order<br>ex: "gMed", "Practice Fusion"  |
| client_mrn                    | String      | An ID the client HMR provides for the patienst medical record number |
| client_name                   | String      | Name of the ordering client |
| client_npi                    | String      | 10-digit NPI number of the ordering customer  |
| client_req_id                 | String      | The clients request id.
| created_at                    | String      | Date the order was created on the service |
| id                            | Number      | Unique identifier of the order (READ ONLY)
| insurance_primary             | Object      | An [insurance object](./post/POST-demographics.md#insurance-object) for the primary insurance payer  |
| insurance_secondary           | Object      | An [insurance object](./post/POST-demographics.md##insurance-object) for the secondary insurance payer  |
| insurance_tertiary            | Object      | An [insurance object](./post/POST-demographics.md##insurance-object) for the primary insurance payer  |
| lis_case_body                 | Object      | The actual case body sent to the Metaclinic API that was used to create the Metaclinic case. |
| lis_case_id                   | Number      | A number that identifies the Metaclinic case that demographics were pushed into. |
| lis_case_number               | String      | The case number of the Metaclinic case that received the demographcis |
| location_name                 | String      | Name of the facility where the procedure was performed  |
| location_npi                  | String      | 10-digit NPI number of the facility where the procedure was performed |
| order_id                      | Number      | ID (foreign key) for the linked "shell" order.  |
| patient                       | Object      | [patient object](./post/POST-demographics.md#patient-object) describing the patient |
| procedure_date                | String      | An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| status                        | String      | A status can be "unmatched", "matchedToOrder" or "deleted"  |
| updated_at                    | String      | Date the order was last updated on the service |



## API Reference

#### Create a new demographic
- [POST/ demographics](./post/POST-demographics.md)

#### Read demographics
- [GET/ demographics](./get/GET-demographics.md)

