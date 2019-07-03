# Orders

## Overview

- [Introduction](#introduction)
- [API Reference](#api-reference)


## Introduction

HMRs and other clients create `orders`. `Orders` are considered "dirty" source
data. The incoming `Order` objects will be conditioned and validated by
app users before becoming `cases` that are ready for import into the LIS.

Before you can access the `orders` resource must first:

- [Register as a user](../registration/readme.md)
- [Receive an authorization token](../users/authenticate.md)
- [Call the desired API endpoints](#api-reference)


## API Reference

### Create an new order
- [`*POST/* orders`](./post/orders.md)

### Read an order
- [`*GET/* orders](./get/orders.md)
- [`*GET/* orders/{id}](./get/order-id.md)
- [`*GET/* order/{id}/{field}`](./get/order-id-field.md)

### Modify an order
- [`*PUT/* orders/{id}`](./put/order-id.md)

### Delete an order
- [`*DELETE/* orders/{id}`](./delete/order-id.md)
