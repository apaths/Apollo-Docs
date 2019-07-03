# Orders

## Overview

- [Introduction](#introduction)
- [API Reference](#api-reference)


## Introduction

HMRs and other clients create `orders` on the server using this API.
`Orders` are considered "dirty" source data. The incoming `Order` objects
will be conditioned as best as possible by the application then presented to
users to validate. After `pending` orders are validated, they become `cases`
that are ready for import into the LIS or any other appliation.

To access the `orders` resource:

- Register as user. See [Registration](../registration/README.md)
- Hold and send an [Authentication token](../authentication/README.md)
- Call [API endpoint](#api-reference) you desire


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
