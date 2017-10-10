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
  -H "token: meowmeowmeow"
```


> Make sure to replace `meowmeowmeow` with your API key.

SafeLock API uses **token** parameter to allow access to the API.

SafeLock API expects for the **token** to be included in all API requests (except Login and Register endpoints) to the server in a header that looks like the following:

`token: meowmeowmeow`

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
        "token": "Ed9vT6zks25TyoV5YEPmd0Nk2ygiIvSUwA6VKyI8vcDcFI9D8zSMwmU4gxyE",
        "id": 2
    }
}
```

This endpoint registers the user and returns a unique **token** with **id** and **email** information for the registered user. This token will be used for identify the user for every request which will be made by a user.

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
            "token": "Ed9vT6zks25TyoV5YEPmd0Nk2ygiIvSUwA6VKyI8vcDcFI9D8zSMwmU4gxyE"
        },
        "fake_passcode": false
    }
}
```

This endpoint returns the user's **token**, **id**, **email** for given credentials and **fake_passcode** login status as bool. If passcode field contains fake passcode then API will recognize it and it will return fake_passcode parameter as true otherwise will return as false.

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

## Create

```shell
curl -X POST "http://138.197.190.67/api/v1.0/albums" -F name="Test album"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "name": "Test album",
        "updated_at": "2017-09-19 07:14:04",
        "created_at": "2017-09-19 07:14:04",
        "id": 7
    }
}
```

This endpoint creates a new album with given name and returns the new album's information if creation was successful.


### HTTP Request

`POST http://138.197.190.67/api/v1.0/albums`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
name | true | Album name (max 255 character)

<aside class="notice">
There is a validation for <b>name</b> field. <b>name</b> fields is required and it can be max 255 characters. If any validation error occurs then <b>"status"</b> key for response will contain <b>"fail"</b> value and <b>"data"</b> key will contain correspond field names with description about the validation errors as an array.
</aside>

## Read

### Get All Albums of User

This will return all albums of the user with **thumb_url** (thumbs are 160x160 and thumbs will be the last photo of the album. if there is no photo on the album then **thumb_url** parameter will be ***null***)

```shell
curl -X GET "http://138.197.190.67/api/v1.0/albums"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": [
        {
            "id": 1,
            "name": "Test albümü 1",
            "created_at": "2017-09-13 13:32:12",
            "updated_at": "2017-09-19 07:36:28",
            "thumb_url": "https://s3.us-east-2.amazonaws.com/safelock/1/1/thumb/300771d9d4566d74e9615096f76dec788881c4b6.jpg"
        },
        {
            "id": 2,
            "name": "Test albümü 2",
            "created_at": "2017-09-13 13:34:05",
            "updated_at": "2017-09-13 13:34:05",
            "thumb_url": null
        }
    ]
}
```

### HTTP Request

`GET http://138.197.190.67/api/v1.0/albums`

### Query Parameters
none

<aside class="notice">
If the user doesn't have any albums then response <b>data</b> key will contain an empty array (<b>[]</b>) and <b>"status"</b> key will be <b>"success"</b>
</aside>

### Get Spesific Album With It's detail

```shell
curl -X GET "http://138.197.190.67/api/v1.0/albums/3"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "id": 1,
        "name": "Thailand",
        "created_at": "2017-10-06 07:44:52",
        "updated_at": "2017-10-06 07:44:52",
        "photos": [
            {
                "id": 1,
                "original_name": "buddha.jpg",
                "hash_name": "361036ff6adb3dd6d51c6d82c0166761ad1a0060.jpg",
                "url": "https://s3.us-east-2.amazonaws.com/safelock/1/1/361036ff6adb3dd6d51c6d82c0166761ad1a0060.jpg",
                "thumb_url": "https://s3.us-east-2.amazonaws.com/safelock/1/1/thumb/361036ff6adb3dd6d51c6d82c0166761ad1a0060.jpg",
                "filesize": "498523",
                "created_at": "2017-10-06 07:47:29"
            },
            {
                "id": 2,
                "original_name": "pattaya.jpg",
                "hash_name": "86da7f1d21d51cb2ef08d755d78dbead0ea7febf.jpg",
                "url": "https://s3.us-east-2.amazonaws.com/safelock/1/1/86da7f1d21d51cb2ef08d755d78dbead0ea7febf.jpg",
                "thumb_url": "https://s3.us-east-2.amazonaws.com/safelock/1/1/thumb/86da7f1d21d51cb2ef08d755d78dbead0ea7febf.jpg",
                "filesize": "182550",
                "created_at": "2017-10-06 07:48:02"
            }
        ]
    }
}
```
This endpoint retrieves one single album with details.


`GET http://138.197.190.67/api/v1.0/albums/{ID}`

### Query Parameters
none

<aside class="notice">
If the album doesn't exist then response <b>"status"</b> key will <b>"fail"</b> and <b>"data"</b> key will contain <b>"There is no such a album"</b>
</aside>

## Update


```shell
curl -X PATCH "http://138.197.190.67/api/v1.0/albums/1" -F name="New name"
```

> The above command returns JSON structured like this:


```json
{
    "status": "success",
    "data": {
        "id": 1,
        "name": "New name",
        "created_at": "2017-09-13 13:32:12",
        "updated_at": "2017-09-19 07:36:28"
    }
}
```

This endpoint updates the album with given id in url and returns updated album's info.


### HTTP Request

`PATCH http://138.197.190.67/api/v1.0/albums/{ID}`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
name | true | Album name (max 255 character)

<aside class="notice">
Like creating albums, updating albums also have same validation.
</aside>

## Delete

```shell
curl -X DELETE "http://138.197.190.67/api/v1.0/albums/5"
````

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": null
}
```

This endpoint deletes the album and it's all photos with given album id in url.

### HTTP Request

`DELETE http://138.197.190.67/api/v1.0/albums/{ID}`

<aside class="notice">
If the album already doesn't exist then response <b>"status"</b> key will <b>"fail"</b> and <b>"data"</b> key will contain <b>"There is no such a album"</b>
</aside>

# Photos

## Create

```shell
curl -X POST "http://138.197.190.67/api/v1.0/photos" -F photo_file=FILE -F album_id="1"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "user_id": 1,
        "album_id": 1,
        "url": "https://s3.us-east-2.amazonaws.com/safelock/1/1/d9b115352f8e0357263f38b8e0941fefc3c56c1d.jpg",
        "thumb_url": "https://s3.us-east-2.amazonaws.com/safelock/1/1/thumb/d9b115352f8e0357263f38b8e0941fefc3c56c1d.jpg",
        "sha1_hash": "d9b115352f8e0357263f38b8e0941fefc3c56c1d",
        "original_name": "5629_10152098364014870_1827540468_n.jpg",
        "filesize": 53243,
        "hash_name": "d9b115352f8e0357263f38b8e0941fefc3c56c1d.jpg",
        "updated_at": "2017-10-03 12:58:33",
        "created_at": "2017-10-03 12:58:33",
        "id": 5
    }
}
```

This endpoint creates a new photo in given album and returns the new photos's information if creation was successful.



### HTTP Request

`POST http://138.197.190.67/api/v1.0/photos`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
album_id | true | Album id that needs to be saved in
photo_file | true | Photo file

<aside class="notice">
There is a special validation for photo_file. If the file already uploaded before then the <b>"status"</b> key will <b>"fail"</b>. And <b>"data"</b> key will contain "You already uploaded this photo before" message.
</aside>

## Delete

```shell
curl -X DELETE "http://138.197.190.67/api/v1.0/photos/5"
````

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": null
}
```

This endpoint deletes the photo with given id in url.

### HTTP Request

`DELETE http://138.197.190.67/api/v1.0/photos/{ID}`

<aside class="notice">
If the photo already doesn't exist then response <b>"status"</b> key will <b>"fail"</b> and <b>"data"</b> key will contain <b>"There is no such a photo"</b>
</aside>

# Notes

## Create

```shell
curl -X POST "http://138.197.190.67/api/v1.0/notes" -F title="My passwords" -F content="Facebook 123456"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "title": "My passwords",
        "content": "Facebook 123456",
        "user_id": 1,
        "updated_at": "2017-10-10 07:31:16",
        "created_at": "2017-10-10 07:31:16",
        "id": 5
    }
}
```

This endpoint creates a new note and returns the new note's information if creation was successful.

### HTTP Request

`POST http://138.197.190.67/api/v1.0/notes`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
title | false | The note's title (optional, max 255 character)
content | true | Note content (required)


<aside class="notice">
There is a validation for <b>content</b> field. <b>content</b> fields is required. If any validation error occurs then <b>"status"</b> key for response will contain <b>"fail"</b> value and <b>"data"</b> key will contain correspond field names with description about the validation errors as an array. <b>title</b> field is optional, there is no validation for this field. If title field is not defined, then the value of title in creation response will contain <i><b>null</b></i>.
</aside>

## Read

### Get All Notes of User

This will return all notes of the user. This endpoint's purpose is listing notes on user side. For this purpose,
note <b>"content"</b> field will be limited with 35 characters in response. This 35 character content field can be used for tableview title or you can use <b>"title"</b> field as tableview title. But remember title field is optional and it can be null.

```shell
curl -X GET "http://138.197.190.67/api/v1.0/notes"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": [
        {
            "id": 1,
            "user_id": 1,
            "title": "Lorem ipsum",
            "content": "Lorem ipsum dolor sit amet, consect...",
            "created_at": "2017-10-10 08:55:57",
            "updated_at": "2017-10-10 08:55:57"
        },
        {
            "id": 2,
            "user_id": 1,
            "title": "Proin facilisis",
            "content": "Proin facilisis tellus lacus, ac lo...",
            "created_at": "2017-10-10 08:56:11",
            "updated_at": "2017-10-10 08:56:11"
        },
        {
            "id": 3,
            "user_id": 1,
            "title": null,
            "content": "Phasellus nec ex volutpat, euismod ...",
            "created_at": "2017-10-10 08:56:23",
            "updated_at": "2017-10-10 08:56:23"
        }
    ]
}
```

### HTTP Request

`GET http://138.197.190.67/api/v1.0/notes`

### Query Parameters
none

<aside class="notice">
If the user doesn't have any notes then response <b>data</b> key will contain an empty array (<b>[]</b>) and <b>"status"</b> key will be <b>"success"</b> Therefore title field can be <b>null</b>. Because this field is optional when creating note.
</aside>



### Get Specific Note of User

This will return specific note of user.

```shell
curl -X GET "http://138.197.190.67/api/v1.0/notes/{ID}"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": {
        "id": 2,
        "user_id": 1,
        "title": "Proin facilisis",
        "content": "Proin facilisis tellus lacus, ac lobortis ipsum euismod vel. Nullam dolor augue, lobortis ut enim nec, blandit blandit dui. Nullam tincidunt lacinia purus, quis rutrum mauris facilisis in. ",
        "created_at": "2017-10-10 08:56:11",
        "updated_at": "2017-10-10 08:56:11"
    }
}
```

### HTTP Request

`GET http://138.197.190.67/api/v1.0/notes/{ID}`

### Query Parameters
none

<aside class="notice">
If the note doesn't exist then response <b>"status"</b> key will <b>"fail"</b> and <b>"data"</b> key will contain <b>"There is no such a note"</b>
</aside>

## Update

```shell
curl -X PATCH "http://138.197.190.67/api/v1.0/notes/1" -F title="New title" content="New content"
```

> The above command returns JSON structured like this:


```json
{
    "status": "success",
    "data": {
        "id": 2,
        "user_id": 1,
        "title": "New Title",
        "content": "New Content",
        "created_at": "2017-10-10 08:56:11",
        "updated_at": "2017-10-10 11:23:35"
    }
}
```

This endpoint updates the note with given id in url and returns updated note's info.


### HTTP Request

`PATCH http://138.197.190.67/api/v1.0/notes/{ID}`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
title | false | Note title (optional, max 255 character)
content | true | Note content (required)


##Delete


```shell
curl -X DELETE "http://138.197.190.67/api/v1.0/notes/2"
````

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "data": null
}
```

This endpoint deletes the note with given id in url.

### HTTP Request

`DELETE http://138.197.190.67/api/v1.0/notes/{ID}`

<aside class="notice">
If the note already doesn't exist then response <b>"status"</b> key will <b>"fail"</b> and <b>"data"</b> key will contain <b>"There is no such a note"</b>
</aside>
