# Registration

## Overview

- [Request with a form](#request-using-a-form)
- [Request with API endpoint](#request-with-api-endpoint)
- [Validation](#validation)
- [Using the API](#using-the-API)

## Request using a form

> NOTE: Future. For now use the direct API endpoint.

You need to register and validate your identity before accessing the API.
Register using the [Sign Up](https://104.37.221.155/register) form. Some basic information
is needed.

## Request with api endpoint

You can also register with a direct call to the an API enpoint. To request an account
make the following call.

**Method:** `POST/` \
**Route:** `/api/v1/users`

Pass the following parameters in the body.

| Parameter        | Type         | Required | Description
|------------------|--------------| :------: |----------------------------------|
| name             | String       | No       | Name for the user                |
| email            | String       | Yes      | Email used for you account       |
| password         | String       | Yes      | Password for your account. Requrires 8 characters. One upper case. One lower case. One number |

> Ensure you send 'Content-Type' header with value 'application/json' when sending
  requests.

## Validation

After completing the form you will recieve a notification email. This lets you
know that we've received your request and APS support will be validating your
identity and creating your account soon.


An APS admin will validate your identity and create your account. You will
receive an email letting you know when this happens. Usually only take
a few minutes.

## Using the API

When validated, you will be able to use the receive authoriztion tokens through
the [Users API](../API/users/README.md). See
[Authentication](../authentication/README.md) for detailed instructions for
authorizing your application.
