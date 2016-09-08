---
title: BOCCO API リファレンス
keywords: BOCCO API, リファレンス, BOCCO
last_updated: September 6, 2016
tags: [reference]
sidebar: reference
permalink: /reference/index.html
folder: reference
---

## API Host

    https://api.bocco.me

## Access Tokenの取得

### Session Collection [/alpha/sessions]

#### Signin [POST]

ユーザの入力したアカウント情報からセッション情報を作成し、Access Tokenを取得します。


+ Parameters

    + apikey (required, string, `xxxxxxxxxxx`) ... Get one from BOCCO support.
    + email (required, string, `email@example.com`) ... Should be a valid email.
    + password (required, string, `xxxxxxxxxxx`) ... Should match the password used when created.

+ Response 200 (application/json)

    A new `access_token` is returned.

    + Body

    {"access_token":"f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6","uuid":"e8bdc076-4045-45a7-abea-b4a34e00a417"}

+ Response 401 (text/plain)

    Server responds with 401 if email doesn't exist or email-password pair doesn't match.

    + Body

    {"code":401001,"message":"Unauthorized"}

## Group Rooms
Rooms resource

### Joined Rooms Collection [/alpha/rooms/joined]

#### Fetch Joined Rooms [GET]

+ Parameters
    + access_token (required, string) ...

+ Response 200 (application/json)

    + Body

    [
      {
        "uuid": "E7607BA3-2AA0-4DEB-8959-E384B3DE82B0",
        "name": "Happy Family",
        "updated_at": "2015-07-31T21:47:46+09:00",
        "background_image": "http://localhost:8000/1/rooms/E7607BA3-2AA0-4DEB-8959-E384B3DE82B0/df5dfb71-9d58-4b50-b82f-eb2524da56da.png",
        "members": [
          {
            "user": {
              "uuid": "eb11c90f-f1a2-43a8-8a12-40406e44005b",
              "user_type": "human",
              "nickname": "5",
              "icon": "",
              "seller": ""
            },
            "joined_at": "2015-05-11T23:52:39+09:00",
            "read_id": 23843
          },
          ...
        ],
        "messages": [
          {
            "id": 24686,
            "unique_id": "1DB34B93-0DFA-4150-AEF5-CBED1F8220C3",
            "date": "2015-07-31T21:47:46+09:00",
            "media": "text",
            "message_type": "normal",
            "user": {
              "uuid": "cffbf787-dd20-4157-8279-f5d287e1783c",
              "user_type": "human",
              "nickname": "mash",
              "icon": "http://localhost:8000/1/users/cffbf787-dd20-4157-8279-f5d287e1783c/d4187679-bd07-49f8-94c9-207f47d641a0.png",
              "seller": ""
            },
            "text": "hoge",
            "audio": "http://localhost:8000/1/messages/24686.ogg",
            "image": "",
            "sender": "cffbf787-dd20-4157-8279-f5d287e1783c",
            "detail": null
          }
        ],
        "sensors": [
          {
            "uuid": "AAA5EC17-7981-4FDC-B783-51BAD4868D38",
            "user_type": "sensor_fire",
            "nickname": "sensor_fire",
            "icon": "",
            "seller": "",
            "address": ""
          },
          ...
        ]
      }
    ]

### Messages in Room Collection [/alpha/rooms/{room_id}/messages]

Messages are ordered by id *ASC* which is the same order as the app's messages view.

+ Parameters
    + room_id (required, string, `2cf4c5b5-8991-46d7-a373-246a9998ab45`) ... uuid of the room.

#### Fetch Messages [GET]

User must have permission to the apikey to access this API. (Get permission from BOCCO support)

+ Parameters
    + newer_than (optional, integer, `1`) ... Response will include messages with `id` parameter newer than (not including) this.
    + older_than (optional, integer, `9`) ... Response will include messages with `id` parameter older than (not including) this.
    + access_token (required, string) ...
    + read (optional, bool, `1`) ... If 1, server will record that the newest message included in the response is read by the client.

+ Response 200 (application/json)

    Messages are ordered by `id` ascending.
    `detail` key's value differs according to `message_type` key.

    + Body

    [
      {
        "id": 60,
        "unique_id": "4ECD2AC5-C9EB-4415-BFAF-BB212F89003A",
        "text": "F",
        "sender": "1bb7bf12-83cf-40fa-9c33-e8580852ba5d",
        "user": {
          "uuid": "1bb7bf12-83cf-40fa-9c33-e8580852ba5d",
          "user_type": "human",
          "nickname": "aopico",
          "icon": "http://localhost:8000/assets/images/23fe1d3a-c1e1-4c56-87ed-502d339460a7.png",
          "seller": ""
        },
        "date": "2014-12-23T14:58:06+09:00",
        "media": "text",
        "audio": "http://localhost:8000/1/messages/60.wav",
        "message_type": "normal",
        "detail": null
      },
      {
        "id": 61,
        "unique_id": "849C4F97-BB2D-4047-8A8E-5035E8025CED",
        "text": "G",
        "sender": "1bb7bf12-83cf-40fa-9c33-e8580852ba5d",
        "user": {
          "uuid": "1bb7bf12-83cf-40fa-9c33-e8580852ba5d",
          "user_type": "human",
          "nickname": "aopico",
          "icon": "http://localhost:8000/assets/images/23fe1d3a-c1e1-4c56-87ed-502d339460a7.png",
          "seller": ""
        },
        "date": "2014-12-23T14:58:38+09:00",
        "media": "text",
        "audio": "http://localhost:8000/1/messages/C4D86E06-2C1F-4672-988F-DD9645DFC19C.wav",
        "message_type": "normal",
        "detail": null
      }
    ]

    + Message format if message_type: system.human_joined or system.sensor_joined

      {
        "id": 110508,
        "unique_id": "4a169f98-065d-4937-aed8-1596919b79fa",
        "date": "2015-07-16T02:54:59Z",
        "media": "text",
        "message_type": "system.human_joined",
        "user": {
          "uuid": "c20807b7-8a4b-44c6-a5b4-19d374a37202",
          "user_type": "bocco",
          "nickname": "BOCCO",
          "icon": "",
          "seller": ""
        },
        "text": " joined our room. Welcome, !",
        "audio": "https://api.bocco.me/1/messages/110508.ogg",
        "image": "",
        "sender": "c20807b7-8a4b-44c6-a5b4-19d374a37202",
        "detail": {
          "joined_user": {
            "uuid": "57b32f63-6398-46b1-aa5f-8780360fbb3e",
            "user_type": "human",
            "nickname": "",
            "icon": "",
            "seller": ""
          }
        }
      }

#### Post a Message [POST]

+ Parameters

    + text (optional, string, `Hi!`) ... Set `media` parameter to `text` when added.
    + audio (optional, binary) ... Binary data of audio file. Set `Content-Type: multipart/form-data` header properly. Set `media` parameter to `audio` when added.
    + image (optional, binary) ... Binary data of image file. Set `Content-Type: multipart/form-data` header properly. Set `media` parameter to `image` when added.
    + unique_id (required, string, `F7827189-E419-4012-820F-4AFD608E1ED2`) ... Unique identifier of message generated on client for idempotence.
    + media (required, string, `text` or `audio` or `image`) ...
    + access_token (required, string) ...

+ Request (multipart/form-data; boundary=BOUNDARY)

    --BOUNDARY
    Content-Disposition: form-data; name="audio"; filename="audio1.wav"
    Content-Type: application/octet-stream

    ...binary...

    --BOUNDARY
    Content-Disposition: form-data; name="access_token"

    f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6
    --BOUNDARY
    Content-Disposition: form-data; name="unique_id"

    55C286C1-FBFC-4523-A5A4-22D33015AA2B
    --BOUNDARY
    Content-Disposition: form-data; name="media"

    audio
    --BOUNDARY--


+ Response 201 (application/json)

    Status code can be 200 if we have already saved that message.

    + Body

    {
      "id":60,
      "unique_id":"4ECD2AC5-C9EB-4415-BFAF-BB212F89003A",
      "text":"F",
      "sender":"1bb7bf12-83cf-40fa-9c33-e8580852ba5d",
      "user":{
        "uuid":"1bb7bf12-83cf-40fa-9c33-e8580852ba5d",
        "user_type":"human",
        "nickname":"aopico",
        "icon":"http://localhost:8000/assets/images/23fe1d3a-c1e1-4c56-87ed-502d339460a7.png",
        "seller":""
      },
      "date":"2014-12-23T14:58:06+09:00",
      "media":"text",
      "audio":"http://localhost:8000/1/messages/60.wav",
      "message_type":"normal"
    }


### Streaming Messages in Room Collection [/alpha/rooms/{room_id}/subscribe]

Messages are ordered by id *ASC* which is the same order as the app's messages view.

+ Parameters
    + room_id (required, string, `2cf4c5b5-8991-46d7-a373-246a9998ab45`) ... uuid of the room.

#### Subscribe to Events [GET]

User must have permission to the apikey to access this API. (Get permission from BOCCO support)

This is a long polling request. Server will respond with a response only if either server has a message newer than provided `newer_than` parameter, or timeout occurs.

Response includes either `message` or `member` events (for now). All events have `event` and `body` keys. Client should branch their code using `event` key.
`message` event format is the same as "Fetch Messages" response format.

+ Parameters
    + newer_than (required, integer, `1`) ... Response will include messages with `id` parameter newer than (not including) this.
    + access_token (required, string) ...
    + read (optional, bool, `1`) ... If 1, server will record that the newest message included in the response is read by the client.

+ Response 200 (application/json)

    + Body (Message event)

    {
      "event": "message",
      "body": {
        "id": 23927,
        "unique_id": "A2B68E53-235E-41EF-9C41-DA75DB897350",
        "date": "2015-06-18T18:08:05+09:00",
        "media": "image",
        "message_type": "normal",
        "user": {
          "uuid": "63686d8f-c07b-431d-bd08-a6bb53a52435",
          "user_type": "human",
          "nickname": "iPhone6",
          "icon": "http://localhost:8000/1/users/63686d8f-c07b-431d-bd08-a6bb53a52435/e3e84fcc-82c9-456f-b2f3-dd1b47028eb7.png",
          "seller": ""
        },
        "text": "Sent a photo",
        "audio": "http://localhost:8000/1/messages/23927.ogg",
        "image": "http://localhost:8000/1/messages/23927.png",
        "sender": "63686d8f-c07b-431d-bd08-a6bb53a52435",
        "detail": null
      }
    }

    + Body (Member event)

    {
      "event": "member",
      "body": {
        "user": {
          "uuid": "63686d8f-c07b-431d-bd08-a6bb53a52435",
          "user_type": "human",
          "nickname": "iPhone6",
          "icon": "http://localhost:8000/1/users/63686d8f-c07b-431d-bd08-a6bb53a52435/e3e84fcc-82c9-456f-b2f3-dd1b47028eb7.png",
          "seller": ""
        },
        "joined_at": "2014-12-21T16:50:41+09:00",
        "read_id": 333
      }
    }

+ Response 408 (text/plain)

    + Body

### Single Message Read Status [/alpha/rooms/{room_id}/messages/{message_id}/read]

#### Report Read Status Update [POST]

User must have permission to the apikey to access this API. (Get permission from BOCCO support)

+ Parameters

    + access_token (required, string, `f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6`) ...

+ Response 200 (application/json)

    + Body

        {}


## Errors

### HTTP status codes

HTTP status codes follow their original meanings, see https://en.wikipedia.org/wiki/List_of_HTTP_status_codes .

+ 401 : access_token is invalid, you're stuck, remove everything and start over from setup.
+ 400 : GET/POST request's parameters are invalid, please check parameters, fix and try again.
+ 404 : Requested resource is not found or not permitted. confirm the resource is accessible.
+ 408 : Request Timeout, check network connection and try again.
+ 429 : Too Many Requests, wait for a while and try again.
+ 500 : Internal Server Error, contact BOCCO support.
+ 502 : Bad Gateway, contact BOCCO support.
+ 503 : Service Unavailable, contact BOCCO support.

### Response body

When HTTP status code is not equal to 200, response body looks like:

    {"code":xxxxxx,"message":xxxxxxxx}

`code` is a error identifier. See each endpoint documentation for details.
`message` is a localized human readable error message.

{% include links.html %}
