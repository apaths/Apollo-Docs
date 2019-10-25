# Shreveport (GIS) Imports

- [Overview](#overview)
- [Data sources and states](#data-sources-and-states)
- [Receive Demographics](#receive-demographics)
- [Receive Partial Orders as PDF](#received-partial-orders)
- [Convert Partial Orders](#convert-partial-order-pdf-to-partial-order-api-objects)
- [Match Demographics to Partial Orders](#match-demographics-to-partial-orders)
- [Create Orders](#create-orders)



## Overview

Shreveport case imports follows of these steps:

* Patient demographics are entered by GIS staff into eClinicalWorks (eCW). eCW
  pushes an HL7 message to our MIRTH server which ETLs the message into a useable
  JSON object. MIRTH server inserts the transformed demographic information into
  the Apollo lander as a [Demographic](../API/demographics/README.md) object.
* Procedure orders are uploaded from GIS' Provation instance via SFTP to the
  `/var/sftp/uploads` folder. The files arrive as *.PDF files. Periodically,
  the Apollo-BOT runs a job that converts the PDF files into JSON files. The JSON
  files are stored in the `/var/www/apollo/server/data/orders` folder. Converted
  PDF files are moved into the `/var/sftp/uploads/archive` folder.
* Periodically the Apoll-BOT ingests each JSON files into a [Partial Order](../API/partialorder/README.md)
  object.
* The Apollo-BOT periodocially runs a job to match any unmatched [Demographic](../API/demographics/README.md)
  objects with any unmatched [Partial Order](../API/partialorder/README.md) objects.
  Any orders that can be matched are then inserted into the [Orders](../API/orders/README.md)
  collection.
* Finally, an APS accessioner then uses the Apollo Lander client appliation to
  review any pending [Orders](../API/orders/README.md). Accessioners access the
  list of pending cases by using the **Shreveport** channel. Accessioners then
  make any corrections and insert the case into the Metaclinic LIS.



## Data sources and states

| Order field              | Source          | Required for LIS creation | Editable in | Description         |
|--------------------------|-----------------| :-----------------------: | :---------: |---------------------|
| client_app               | eCW             | No                        | None        | used for audit only. |
| client_req_id            | not used        | No                        | Both        |                      |
| client_mrn               | Both            | Yes                       | Neither     | Matching criteria    |
| client_name              | Provation       | Yes                       | Shell case  |                      |
| client_npi               | Provation       | Yes                       | Shell case  |                      |
| data                     | eCW+Provation   | No                        | None        | Hidden, used for audit only. |
| diagnosis_comment        | Provation       | No                        | Shell case  | (not typically used) |
| diagnosis_history        | Provation       | Yes                       | Shell case  | Impressions and indications fields |
| diagnosis_post_op        | ?               | No                        | Shell case  | Dont' see any diagnosis codes on either system |
| diagnosis_pre_op         | ?               | No                        | Shell case  | Don't see a source   |
| instructions             | ?               | No                        | Shell case  | (not typically used) |
| insurance_primary.*      | eCW             | No                        | Demo        |                      |
| insurance_secondary.*    | eCW             | No                        | Demo        |                      |
| insurance_tertiary.*     | eCW             | No                        | Demo        |                      |
| location_name            | Provation       | Yes                       | Shell case  |                      |
| location_npi             | Provation       | Yes                       | Shell case  | (fixed)              |
| patient.address_street_1 | eCW             | No                        | Demo        |                      |
| patient.address_street_2 | eCW             | No                        | Demo        |                      |
| patient.city             | eCW             | No                        | Demo        |                      |
| patient.country_code     | eCW             | No                        | Neither     | Default to 'US' for now  |
| patient.date_of_birth    | eCW             | No                        | Demo        |                      |
| patient.email            | eCW             | No                        | Demo        |                      |
| patient.first_name       | Both            | Yes (see note)            | Both        | LIS requires patient.firstName to create a case. Use Provation first name, updateable when demographics are available. |
| patient.last_name        | Both            | Yes (see note)            | Both        | LIS requires patient.lastName to create a case. Use Provation initially, then updateable when demographics are received. |
| patient.middle_name      | eCW             | No                        | Demo        |                      |
| patient.phone_number     | eCW             | No                        | Demo        |                      |
| patient.prefix           | eCW             | No                        | Demo        |                      |
| patient.sex              | eCW             | No                        | Demo        |                      |
| patient.ssn              | eCW             | No                        | Demo        |                      |
| patient.state            | eCW             | No                        | Demo        |                      |
| patient.suffix           | eCW             | No                        | Demo        |                      |
| patient.zip              | eCW             | No                        | Demo        |                      |
| physician_primary        | Provation       | Yes (see note)            | Shell case  | LIS does not require physician, but we should. Only the allow the physician shown on the Provation order. |
| physician_secondary      | Provation       | No                        | Shell case  | Only allow the referring physicain listed on teh Provation req.  |
| procedure_date           | Provation       | Yes                       | Shell case  |                      |
| procedure_type           | Provation       | Yes                       | Shell case  | Look for matching MRN/Date records with differing procedure_types. Union these and append bottles to a common sample list. |
| screening                | ?               | No                        | Both        | Havent' seen a data source for this? |
| specimens                | Provation       | Yes                       | Shell case  | Handle doubles correctly with a union |
| stat                     | ?               | No                        | Both        | Haven't seen a data source for this? |



## Receive Demographics

Coming soon.



## Receive Partial Orders as PDF

Coming soon.



## Convert Partial Orders PDF to Partial Order API objects

Coming soon.



## Match Demographics to Partial Orders

Coming soon.



## Create Orders

Coming soon.
