# Shreveport (GIS) Imports

- [Overview](#overview)
- [Receive Demographics](#receive-demographics)
- [Receive Partial Orders as PDF](#received-partial-orders)
- [Convert Partial Orders](#convert-partial-order-pdf-to-partial-order-api-objects)
- [Match Demographics to Partial Orders](#match-demographics-to-partial-orders)
- [Create Orders](#create-orders)

## Overview

Shreveport case imports consists of these steps:

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



## Receive Demographics

Coming soon.



## Receive Partial Orders as PDF

Coming soon.



## Convert Partial Orders PDF to Partial Order API objects

Coming soon.



## Match Demographics to Partial Orders

Coming soon...



## Create Orders
