# Orders

## Overview

- [Introduction](#introduction)
- [Fields](#fields)
- [API Reference](#api-reference)
  - [Create a new Shell Order](#create-a-new-shell-order)
  - [Read Shell Orders](#read-shell-orders)
  - [Update an Existing Shell Order](#update-an-existing-shell-order)
  - [Delete a Shell Order](#delete-a-shell-order)



## Introduction

HMRs and other clients create `shellorders` on the server using this API.
Shellorders are [Orders](../orders/README.md), but, have no demographic
infomration. Shellorders rely on a matching [Demographic](../demographics/README.md)
to form a complete order.

The purpose of ShellOrders is to allow asynchronously receive procedure and
patient informatin from different data sources, then combine them as they
become available. One example use of ShellOrders is the [Shreveport Channel](../../shreveport/README.md). The sequence of assembly of
ShellOrders is the described in the [Shereveport Channel Overview](../../shreveport/README.md#overview).

Like `Orders`... `ShellOrders` are considered "dirty" source data. The incoming
`ShellOrder` objects are conditioned as best as possible by the application
then presented to users to validate. After `pending` ShellOrders are validated,
they become `cases` that are ready for import into the LIS or any other
application.

To access the `ShellOrders` resource:

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
| client_mrn                    | String      | An ID the client HMR provides for the patienst medical record number |
| client_name                   | String      | Name of the ordering client |
| client_npi                    | String      | 10-digit NPI number of the ordering customer  |
| created_at                    | String      | Date the order was created on the service |
| data                          | Object      | A structured JSON object for any optional data the interface needs to provide to the case object. Use this for any information that doesnt fit into the standard fields. |
| demographics_id               | Number      | An ID (foreign key) of a matching [Demographics](../demographics/README.md) object  |
| diagnosis_comments            | String      | Any additional comments |
| diagnosis_history             | String      | Physician supplied diagnosis history. Used to provide background to reading pathologist. |
| diagnosis_post_op             | String      | Physician supplied diagnosis information created after the procedure.|
| diagnosis_pre_op              | String      | Physician supplied diagnosis information created prior to the procedure being performed   |
| id                            | Number      | Unique identifier of the order (READ ONLY)
| lis_case_id                   | Number      | `caseID` of the Metaclinic LIS case created from this `Order`.  |
| lis_case_number               | String      | `caseNumber` of the Metalclnic LIS case created fro this `Order`. |
| location_name                 | String      | Name of the facility where the procedure was performed  |
| location_npi                  | String      | 10-digit NPI number of the facility where the procedure was performed |
| patient                       | Object      | [patient object](./post/POST-orders.md#patient-object) describing the patient. Note that the patient fields duplicate fields available in the [Demographics](../demographics/README.md). All patient data in the Shell Case may be overwritten at any time by the Demographics[Demographics](../demographics/README.md) file. See [Data sources and state](../shrevepot/README.md) for more detail. |
| physician_primary             | Object      | A [physician object](./post/POST-orders.md#physician-object) for the primary (ordering) physician  |
| physician_secondary           | Object      | A [physician object](./post/POST-orders.md#physician-object) for the secondary (referring) physician  |
| procedure_date                | String      | An ISO8601 date/time stamp. If no time is provided it will be recorded as midnight of the supplied day in the US-CT timezone  |
| procedure_type                | String      | Type of procedure. Typcally ("EGD", "Colon", "Double"). |
| screening                     | Boolean     | Indicates procedure is screening or not  |
| specimens                     | Array       | An array of [specimen objects](./post/POST-orders.md#specimen-object) submitted for pathology  |
| stat                          | Boolean     | Indicates if order requires stat testing   |
| status                        | String      | Indicates status. One of `pending`, `converted`, `deleted` |
| updated_at                    | String      | Date the order was last updated on the service |

## API Reference

### Create a new Shell Order
- [POST/ orders](./post/POST-shellorders.md)

### Read Shell Orders
- [GET/ orders](./get/GET-shellorders.md)

### Update an Existing Shell Order
- [PATCH/ orders/{id}](./put/PATCH-shellorders-id.md)

### Delete a Shell Order
- [DELETE/ orders/{id}](./delete/DELETE-shellorders-id.md)
