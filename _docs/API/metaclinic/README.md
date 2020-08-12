# Metaclinic

## Overview

- [Introduction](#introduction)
- [API Reference](#api-reference)


## Introduction

The Metaclinic service allows you to interact with the LIMS API. Metaclinic's API
does not allow CORS from browsers. This isn't an issue from non-browser
clients. So we use direct calls from our node applications.


## API reference

### Authentication

#### Get an access token
- [POST/ metaclinic/token](./post/metaclinic-token.md)

### Cases

#### Push a case into LIS
- [POST/ metaclinic/cases](./post/GET-cases.md)

#### Fetch a case from the LIS
- [GET/ metaclinic/cases](./get/GET-cases.md)

#### Check if a case exists in the LIS
- [GET/ metaclinic/casecheck](./get/GET-casecheck.md)

### Lookup data
- [POST/ metaclinic/lookupdata](./post/metaclinic-lookupdata.md)

