# Orders

## Overview

- [Introduction](#introduction)
- [Fields](#fields)
- [API Reference](#api-reference)


## Introduction

HMRs and other clients create `orders` on the server using this API.
`Orders` are considered "dirty" source data. The incoming `Order` objects
will be conditioned as best as possible by the application then presented to
users to validate. After `pending` orders are validated, they become `cases`
that are ready for import into the LIS or any other appliation.

To access the `orders` resource:

- Register as user. See [Registration](../../registration/README.md)
- Hold and send an [Authentication token](../../authentication/README.md)
- Call [API endpoint](#api-reference) you desire

## Fields

The orders services exposes these fields through the API. These fields are queryable
via the [GET/](./get/GET-orders.md) route. Specify the desired fields in the `fields`
parameter.

| Field                         | Type        | Description                        |
|-------------------------------|-------------|------------------------------------|
| client_app                    | String      | Name of the HMR or client appliation generating the order<br>ex: "gMed", "Practice Fusion"  |
| client_record_id              | String      | An ID the client HMR uses to track the order. |
| client_medical_record_number  | String      | An ID the client HMR provides for the patienst medical record number |
| client_name                   | String      | Name of the ordering client |
| client_npi                    | String      | 10-digit NPI number of the ordering customer  |
| created_at                    | String      | Date the order was created on the service |
| data                          | Object      | A structured JSON object for any optional data the interface needs to provide to the case object. Use this for any information that doesnt fit into the standard fields. |
| diagnosis_comments            | String      | Any additional comments |
| diagnosis_history             | String      | Physician supplied diagnosis history. Used to provide background to reading pathologist. |
| diagnosis_post_op             | String      | Physician supplied diagnosis information created after the procedure.|
| diagnosis_pre_op              | String      | Physician supplied diagnosis information created prior to the procedure being performed   |
| id                            | Number      | Unique identifier of the order (READ ONLY)
| instructions                  | String      | Physician instructions sent to the lab |
| insurance_primary             | Object      | An [insurance object](./post/POST-orders.md#insurance-object) for the primary insurance payer  |
| insurance_secondary           | Object      | An [insurance object](./post/POST-orders.md##insurance-object) for the secondary insurance payer  |
| insurance_tertiary            | Object      | An [insurance object](./post/POST-orderes.md##insurance-object) for the primary insurance payer  |
| imported_at                   | String      | Date order was imported into the resource  |
| location_name                 | String      | Name of the facility where the procedure was performed  |
| location_npi                  | String      | 10-digit NPI number of the facility where the procedure was performed |
| patient                       | Object      | [patient object](./post/POST-orders.md#patient-object) describing the patient |
| physician_primary             | Object      | A [physician object](./post/POST-orders.md#physician-object) for the primary (ordering) physician  |
| physician_secondary           | Object      | A [physician object](./post/POST-orders.md#physician-object) for the secondary (referring) physician  |
| procedure_date                | String      | An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| procedure_type                | String      | Type of procedure. Typcally ("EGD", "Colon", "Double"). |
| specimens                     | Array       | An array of [specimen objects](./post/POST-orders.md#specimen-object) submitted for pathology  |
| status                        | String      | Indicates status. One of `pending`, `converted`, `deleted`  |
| updated_at                    | String      | Date the order was last updated on the service |

## API Reference

### Create a new order
- [POST/ orders](./post/POST-orders.md)

### Read orders
- [GET/ orders](./get/GET-orders.md)
- [GET/ orders/{id}](./get/GET-orders-id.md)
- [GET/ orders/{id}/{field}](./get/GET-orders-id-field.md)

### Update an order
- [PUT/ orders/{id}](./put/PUT-orders-id.md)

### Delete an order
- [DELETE/ orders/{id}](./delete/DELETE-orders-id.md)
