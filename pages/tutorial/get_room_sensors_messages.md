---
title: 部屋センサの情報を取得する
keywords: BOCCO API, リファレンス, BOCCO, 部屋センサ
last_updated: November 2, 2017
tags: [tutorial]
toc_at_sidebar: true
permalink: /get_room_sensors_messages.html
folder: tutorial
summary: BOCCO API を使って、部屋センサの情報を取得する方法を学びます。
---

## 準備

### アクセストークンの取得

API リクエストに、 `access_token` が必要です。   
[アクセストークンの取得](/get_access_token.html) の手順に従い、`access_token` を事前に取得してください。

### room_id、sensor_id の取得

部屋センサが結びついているチャットルームを指定するために、 `room_id` が必要です。また，部屋センサを指定するために、 `sensor_id` が必要です．

[チャットルームの取得](/get_joined_rooms.html) の手順に従いチャットルームの情報を取得し、チャットルームの `uuid` (以後 `room_id` として使う)と
部屋センサの `uuid` (以後 `sensor_id` として使う)を事前に取得してください．

例として、下記のようなチャットルームの情報が取得できた場合、 `room_id`は2行目の `WWWWWW-XXXXXXX-YYYYYY-ZZZZZ`、
`sensor_id` は41行目の `AAAAAAA-BBBB-CCCC-DDDDDDD` となります。

```json
{
        "uuid": "WWWWWW-XXXXXXX-YYYYYY-ZZZZZ", # チャットルームのID
        "name": "BOCCO house",
        "updated_at": "2017-11-02T07:03:26Z",
        "background_image": "",
        "members": [
            {
                "user": {
                    "uuid": "XXXXXXXXX-XXXXXXXXX-XXXXXXXX-XXXXXXXX",
                    "user_type": "human",
                    "official": false,
                    "nickname": "Yukai Taro",
                    "icon": "https://api.bocco.me/alpha/users/XXXX.png",
                    "seller": ""
                },
                "joined_at": "2017-11-02T07:00:46Z",
                "read_id": 2930642
            },
            {
                "user": {
                    "uuid": "YYYYYY-YYYYYYYYY-YYYYYYYY-YYYYYYYYy",
                    "user_type": "bocco",
                    "official": false,
                    "nickname": "BOCCO",
                    "icon": "",
                    "seller": "dmm"
                },
                "joined_at": "2017-11-02T06:59:20Z",
                "read_id": 0
            }
        ],
        "messages": [
            {
            .
            .
            .
            }
        ],
        "sensors": [
            {
                "uuid": "AAAAAAA-BBBB-CCCC-DDDDDDD", # 部屋センサのID
                "user_type": "sensor_room", # 部屋センサ
                "official": false,
                "nickname": "Room Sensor",
                "icon": "",
                "seller": "",
                "address": "ffffff"
            }
        ]
    },
.
.
.
```

## リクエスト

curlを使った場合のリクエスト例です。

`room_id` として、準備で取得した `WWWWWW-XXXXXXX-YYYYYY-ZZZZZ` を使います。
`sensor_id` として、同じく準備で取得した `AAAAAAA-BBBB-CCCC-DDDDDDD` を使います。

リクエストBody

- access_token : 取得したアクセストークン。

```bash
curl -i "https://api.bocco.me/alpha/rooms/WWWWWW-XXXXXXX-YYYYYY-ZZZZ/sensor_environments/AAAAAAA-BBBB-CCCC-DDDDDDD?access_token=68b02edexxxxxxxxxxxxxxxxxxxxxx"
```

## レスポンス

### 成功

正しい情報で送信すれば、JSONで部屋センサ情報オブジェクトの配列が返ってきます。

レスポンスHeaders

```
HTTP/1.1 200 OK
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 02 Nov 2017 08:30:42 GMT
Transfer-Encoding: chunked
Via: 1.1 vegur
```

レスポンスBody

```json
[
    {
        "id": 944053,
        "unique_id": "d88a4489-d1c5-40d1-9584-c6e6cbcc4c87",
        "created_at": "2017-11-02T07:08:01Z",
        "illuminance": 5.12,
        "temperature": 24,
        "humidity": 48
    },
    {
        "id": 944018,
        "unique_id": "69932fc7-efb0-4a98-9ecf-80a48b4fd16a",
        "created_at": "2017-11-02T07:03:26Z",
        "illuminance": 5.12,
        "temperature": 24,
        "humidity": 49
    },
    .
    .
    .
```

### 失敗

`room_id`、`sensor_id`が間違っていると，404が返ります。

レスポンスHeaders

```
HTTP/1.1 404 Not Found
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 02 Nov 2017 08:35:04 GMT
Content-Length: 37
Via: 1.1 vegur
```

レスポンスBody

```
{
    "code" : 404001,
    "message" : "Not Found"
}
```

`access_token` が間違っていると、401が返ります。

レスポンスHeaders

```
HTTP/1.1 401 Unauthorized
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 08 Sep 2016 09:03:25 GMT
Content-Length: 43
Via: 1.1 vegur
```

レスポンスBody

```json
{
    "code" : 401001,
    "message" : "認証エラー"
}
```

これで「部屋センサの情報を取得する」のチュートリアルを終わります。
さらに詳細が知りたい場合は、[リファレンス(部屋センサの情報取得)](/reference.html#get-alpharoomsroomidsensorenvironmentssensorid) をご覧ください。
