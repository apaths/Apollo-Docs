# APS Case Intake App

## Overview

- [Introduction](#introduction)
- [Documentation](#documentation)

## Introduction

The APS Case Intake App provides a landing spot for orders generated from
HMRs, or any other client software.

It exposes a [REST API](./_docs/API/README.md) to external applications.
The API consists of four main resources.

| Resource      | Description                                                  |
|---------------|--------------------------------------------------------------|
| Cases         | Supports outbound requests for cases ready to import to LIS  |
| Orders        | Provides interface for creating, reading, updating orders    |
| Pending       | Supports a client application for users to review and verify orders  |
| Users         | Supports user autorizatin and management                     |

## Documentation

- [Registration](./_docs/registration/README.md)
- [Authorization](./_docs/soon.md)
- [API Reference](./_docs/API/README.md)
  - [Orders](./_docs/API/orders/README.md)
  - [Pending](./_docs/soon.md)
  - [Cases](./_docs/soon.md)
  - [Users](./_docs/soon.md)




