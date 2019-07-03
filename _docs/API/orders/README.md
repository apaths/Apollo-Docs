# Orders

## Overview

- [Introduction](#introduction)
- [API Reference](#api-reference)


## Introduction

HMRs and other clients create `orders`. `Orders` are considered "dirty" source
data. The incoming `Order` objects will be conditioned and validated by
app users before becoming `cases` that are ready for import into the LIS.

Before you can access the `orders` resource must first:

- [Register as a user](../registration/README.md)
- [Receive an authorization token](../authentication/README.md)
- [Call the desired API endpoints](#api-reference)


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
