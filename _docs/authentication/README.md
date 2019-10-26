# Authentication

## Overview

- [Introduction](#introduction)
- [Request a token](#request-a-token)
  - [Responses](#responses)
- [Include a token in API requests](include-a-token-in-api-requests)


## Introduction

You need a token to access API routes. You must be a
[registered user](../registration/readme.md) and have been verified by an APS
adminstrator to successfully [request a token](#request-a-token).


## Request a token

If you're a [registered user](../registration/readme.md) you can request a token
with by submitting your email and password to the `/apitoken` route. Call the
route as follows:

**Method:** `POST/` \
**Route:** `/api/v1/users/apitoken`

Pass the following parameters in the body.

| Parameter        | Type         | Required | Description
|------------------|--------------| :------: |----------------------------------|
| email            | String       | Yes      | Email used for you account       |
| password         | String       | Yes      | Password for your account        |

### Responses

`200 SUCCESSFUL`
Body will include an object with a token.

``` Javascript
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5..."
}
```

`401 UNAUTHORIZED`
Body will include an object with a message describing why the request failed.

``` Javascript
{
  message: "Unknown username or email"
}
```

`500 SERVER ERROR`
Indicates a server fault has occurred.


## Include a token in API requests

You need to include the access token in an `Authorizion` request header, as a
`Bearer` token.

For example in Postman, you can submit your token as a header with the following:

**Key**: Authorization
**Value**: "Bearer eyJhbGciOiJIUzI1NiIsInR5..."


