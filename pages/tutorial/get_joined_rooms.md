---
title: チャットルームを取得する
keywords: BOCCO API, リファレンス, BOCCO
last_updated: September 7, 2016
tags: [tutorial]
toc_at_sidebar: true
permalink: /get_joined_rooms
folder: tutorial
summary: BOCCO API を使って、自分が入っているチャットルームの一覧を取得する方法を学びます。
---

## 準備

### アクセストークンの取得

API リクエストに、 `access_token` が必要です。   
[アクセストークンの取得](/get_access_token.html) の手順に従い、`access_token` を事前に取得してください。


## リクエスト

curl を使った場合のリクエスト例です。

- apikey : API申し込み時に送られてきたAPIキー。
- email: ユーザ登録時に入力したメールアドレス。
- password: ユーザ登録時に入力したパスワード。

```bash
curl -i "https://api.bocco.me/alpha/rooms/joined?access_token=f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6"
```

## レスポンス

### 成功

正しいアクセストークンを使用すれば、 JSON でチャットルームの配列が返ってきます。  
`uuid` は、以降のチュートリアルで値を使用します。

レスポンスHeaders

```
HTTP/1.1 200 OK
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 08 Sep 2016 08:38:53 GMT
Transfer-Encoding: chunked
Via: 1.1 vegur
```

レスポンスBody

```json
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
```

### 失敗

`access_token` が誤っていると、 401 が返ります。

レスポンスHeaders

```
HTTP/1.1 401 Unauthorized
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 08 Sep 2016 08:40:55 GMT
Content-Length: 40
Via: 1.1 vegur```

レスポンスBody

```json
{
    "code" : 401001,
    "message" : "Unauthorized"
}
```

これで「チャットルームを取得する」のチュートリアルを終わります。  
さらに詳細が知りたい場合は、[リファレンス(チャットルームの取得)](/reference.html#get-roomsjoined) をご覧ください。

次の項目は、「[メッセージを送信する](/post_message.html)」です。
