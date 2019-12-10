# Shell Orders



## Overview





## Description

Shell orders are just a regular [Order](../API/orders/README.md) object with
the property `is_shell_order` set to `true`. A "shell" [Order](../API/orders/README.md)
only contains the minimal amount of information to create a case in the LIS.

If an [Order](../API/orders/README.md) is designated as a shell order the
demogrpahic infomration won't be located within that order, but rather in an
external [Demographics](../API/demographcis/README.md) object. Paired to gether
they make a complete order. The [Order](../API/orders/README.md) store a link
in the `demog_id` propert to indicate the linked [Demographic](../API/demographics/README.md)
object. A value `null` indicate the order has not been linked, yet.



## Pairing Methods



## Automated Pairing Jobs






