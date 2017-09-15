---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the SafeLock API! You can use our API to access SafeLock API endpoints, which can get information on various albums, photos, and notes in our database.

This API uses JSend specification for response structure. Please take a few minutes to review
 [JSend](https://labs.omniti.com/labs/jsend)

We have language bindings in Shell. You can view code examples in the dark area to the right.


# Authentication

> Example request with curl:


```shell
# With shell, you can just pass the correct header with each request
curl "http://138.197.190.67/api/v1.0/{some_resource_needs_auth}"
  -H "api_token: meowmeowmeow"
```


> Make sure to replace `meowmeowmeow` with your API key.

SafeLock API uses **api_token** parameter to allow access to the API.

SafeLock API expects for the **api_token** to be included in all API requests (except Login and Register endpoints) to the server in a header that looks like the following:

`api_token: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your user API key.
</aside>

## Registration

```shell
curl -X POST "http://138.197.190.67/api/v1.0/register" -F email=furkan@furkan.com -F passcode=1111
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "email": "furkan@furkan.com",
        "api_token": "Ed9vT6zks25TyoV5YEPmd0Nk2ygiIvSUwA6VKyI8vcDcFI9D8zSMwmU4gxyE",
        "id": 2
    }
}
```

This endpoint registers the user and returns a unique **api_token** with **id** and **email** information for the registered user. This token will be used for identify the user for every request which will be made by a user.

### HTTP Request

`POST http://138.197.190.67/api/v1.0/register`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
email | true | The user's email address
passcode | true | The user's passcode (must be 4 digits)

<aside class="notice">
There is a validation for <b>email</b> and <b>passcode</b> fields. <b>passcode</b> field must be 4 digits. If any validation error occurs then <b>"status"</b> key for response will contain <b>"fail"</b> value and <b>"data"</b> key will contain correspond field names with description about the validation errors as an array.
</aside>


## Login

```shell
curl -X POST "http://138.197.190.67/api/v1.0/login" -F email=furkan@furkan.com -F passcode=1111
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "user": {
            "id": 2,
            "email": "furkan@furkan.com",
            "api_token": "Ed9vT6zks25TyoV5YEPmd0Nk2ygiIvSUwA6VKyI8vcDcFI9D8zSMwmU4gxyE"
        },
        "fake_passcode": false
    }
}
```

This endpoint returns the user's **api_token**, **id**, **email** for given credentials and **fake_passcode** login status as bool. If passcode field contains fake passcode then API will recognize it and it will return fake_passcode parameter as true otherwise will return as false.

### HTTP Request

`POST http://138.197.190.67/api/v1.0/login`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
email | true | The user's email address
passcode | true | The user's passcode <b>or</b> fake passcode (must be 4 digits)

<aside class="notice">
There is a validation for <b>email</b> and <b>passcode</b> fields. <b>passcode</b> field must be 4 digits. If any validation error occurs then <b>"status"</b> key for response will contain <b>"fail"</b> value and <b>"data"</b> key will contain correspond field names with description about the validation errors as an array.
</aside>

# Albums

## Will be here soon..
