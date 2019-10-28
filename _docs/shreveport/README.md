# Shreveport (GIS) Imports

- [Overview](#overview)
- [Receive Demographics](#receive-demographics)
- [Receive Partial Orders as PDF](#received-partial-orders)
- [Convert Partial Orders](#convert-partial-order-pdf-to-partial-order-api-objects)
- [Match Demographics to Partial Orders](#match-demographics-to-partial-orders)
- [Create Orders](#create-orders)
- [Data sources and states](#data-sources-and-states)



## Overview

Shreveport (GIS) cases arrive in two parts. First, an electronic
requisition arrives from the Gastroenterologist via Provation. Separately,
and typically (but not necessarily) patient demographics arrive via
eClinicalWorks. The combination of these two components complete the
information needed to create, process, and report a case in the Metaclinic
LIS.

Assembling a complete case follows of these steps:

* Patients receive a procedure (Upper GI endoscopy, or Colonoscopy) or both
  procedures.
  - When preforming the procedures GIS staff enter the lab request in Provation.
  - Staff print the order and the Black Ice print driver makes a PDF that lands
    in the `var/sftp/uploads/archive` folder. **One PDF is created for *each*
    procedure. If the patient recieved both procedures (a "double"), two PDFs
    are created, and differentiated in the `Procedure Type` field.**

* Periodically the Apollo-BOT runs processes each PDF requisition. It...
  - Parses the PDF file into a useable JavaScript object.
  - Creates a ["Shell" Order](../shellorders/README.md) object with property `is_shell_order` set to `true`.
  - Optionally writes the JavaScript object to a JSON file object in the
    `server/data/shellorders/` folder for troubleshooting. Configuration
    property `shellOrders.saveJSON` toggles this feature.

* A Lab user then reviews the ["Shell" Order](../shellorders/README.md) using
  the [Apollo Client](https://github.com/apaths/apollo-client). When they are
  satisfied with the values they use the tool to insert the shell order into
  the Shreveport (GIS) instance of the Metaclinic LIS. **At this point the case
  may proceed through the lab processing portion of the Metaclinic workflow**

* At some (asynchronous) time demographic information is received from
  eClinicalWorks (eCW) via the MIRTH server. Incoming messages are pushed
  into a [Demographic](../API/demographics/README.md) object.

* Periodically the Apollo-BOT runs a job that:
  - Pairs any unmatched [Demographic](../API/demographics/README.md) records
    to any ["Shell" Order](../shellorders/README.md).
  - Pairs any unmatched [Demographic](../API/demographics/README.md) records
    to any cases that have been ingested into the APS instance of the
    Metaclinic LIS. *Note: we use the MRN and the accessionNumber to make this
    match.*

* An APS or Shrevport(GIS) accessioner then reviews the demographic
  information received in the [Apollo Client](https://github.com/apaths/apollo-client).
  When satisfied with the results, they can send the demographic infomraton to
  the Shrevport LIS instance, and/or the APS LIS instance. When sending to the
  APS LIS we warn the user that the operation destroys any changes to the
  fields made in LIS.

  **Demographics cannot create a case, only modify an existing case.**

After completing these steps we will have a complete Case object in the
downstream Metaclinic LIS.

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




