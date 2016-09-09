---
title: メッセージを取得する
keywords: BOCCO API, リファレンス, BOCCO
last_updated: September 7, 2016
tags: [tutorial]
toc_at_sidebar: true
permalink: /get_messages.html
folder: tutorial
summary: BOCCO API を使って、メッセージを取得する方法を学びます。
---

## 準備

### アクセストークンの取得

API リクエストに、 `access_token` が必要です。   
[アクセストークンの取得](/get_access_token.html) の手順に従い、`access_token` を事前に取得してください。

### room_id の取得

メッセージを送信するチャットルームを指定するために、 `room_id` が必要です。
[チャットルームの取得](/get_joined_rooms.html) の手順に従い、`uuid` (以後 `room_id` として使う) を事前に取得してください。

## 範囲を指定せずに取得

### リクエスト

curl を使った場合のリクエスト例です。  

`room_id` として、チャットルームの取得で返ってきた `E7607BA3-2AA0-4DEB-8959-XXXXXXXXXXXX` を使用します。


```bash
curl -i "https://api.bocco.me/alpha/rooms/E7607BA3-2AA0-4DEB-8959-XXXXXXXXXXXX/messages?access_token=x2c4sjxacw4tq72q5h9vrsgjbcz3p55ixxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```


### レスポンス

#### 成功

正しいアクセストークンとチャットルームuuidを使用すれば、 JSON でチャットルームの配列が返ってきます。  

レスポンスHeaders

```
HTTP/1.1 200 OK
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 08 Sep 2016 09:22:03 GMT
Transfer-Encoding: chunked
Via: 1.1 vegur
```

レスポンスBody

```json
[
  {
    "id": 60,
    "unique_id": "4ECD2AC5-C9EB-4415-BFAF-ZZZZZZZZZZZZ",
    "text": "こんにちはBOCCO！",
    "sender": "1bb7bf12-83cf-40fa-9c33-xxxxxxxxxxxx",
    "user": {
      "uuid": "1bb7bf12-83cf-40fa-9c33-xxxxxxxxxxxx",
      "user_type": "human",
      "nickname": "aopico",
      "icon": "http://localhost:8000/assets/images/23fe1d3a-c1e1-4c56-87ed-000000000000.png",
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
    "unique_id": "849C4F97-BB2D-4047-8A8E-YYYYYYYYYYYY",
    "text": "おはよう",
    "sender": "1bb7bf12-83cf-40fa-9c33-xxxxxxxxxxxx",
    "user": {
      "uuid": "1bb7bf12-83cf-40fa-9c33-xxxxxxxxxxxx",
      "user_type": "human",
      "nickname": "aopico",
      "icon": "http://localhost:8000/assets/images/23fe1d3a-c1e1-4c56-87ed-000000000000.png",
      "seller": ""
    },
    "date": "2014-12-23T14:58:38+09:00",
    "media": "text",
    "audio": "http://localhost:8000/1/messages/C4D86E06-2C1F-4672-988F-000000000000.wav",
    "message_type": "normal",
    "detail": null
  }
]
```

#### 失敗

`access_token` が誤っていると、 401 が返ります。

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
    "message" : "Unauthorized"
}
```

## 範囲を指定して取得

先ほどのレスポンスでは、最新の id として 61 が取得出来ました。  
次に、61より新しいメッセージのみを取得してみましょう。

### リクエスト

先ほどのリクエストのクエリストリングに `newer_than` を追加します。

- newer_than : 61 を指定します。この時、id:61 は含まれない点に注意してください。


```bash
curl -i "https://api.bocco.me/alpha/rooms/E7607BA3-2AA0-4DEB-8959-XXXXXXXXXXXX/messages?access_token=x2c4sjxacw4tq72q5h9vrsgjbcz3p55ixxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&newer_than=61"
```

### レスポンス

```json
[]
```

空のレスポンスが返ってきましたか？先ほどメッセージを取得してから新しいメッセージがなければ、このようなレスポンスが返って来ます。

[メッセージの送信](/post_message.html) のように、1件新しいメッセージを送信してみましょう。

### 再度リクエスト 

メッセージを送信したら、先ほどと同じ内容で再度メッセージを取得してみます。

```bash
curl -i "https://api.bocco.me/alpha/rooms/E7607BA3-2AA0-4DEB-8959-XXXXXXXXXXXX/messages?access_token=x2c4sjxacw4tq72q5h9vrsgjbcz3p55ixxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&newer_than=61"
```

### レスポンス

```json
[
  {
    "id": 70,
    "unique_id": "DC1F23A2-7571-4CBE-8364-XXXXXXXXXXXX",
    "text": "こんばんは",
    "sender": "1bb7bf12-83cf-40fa-9c33-xxxxxxxxxxxx",
    "user": {
      "uuid": "1bb7bf12-83cf-40fa-9c33-xxxxxxxxxxxx",
      "user_type": "human",
      "nickname": "aopico",
      "icon": "http://localhost:8000/assets/images/23fe1d3a-c1e1-4c56-87ed-000000000000.png",
      "seller": ""
    },
    "date": "2014-12-23T15:30:13+09:00",
    "media": "text",
    "audio": "http://localhost:8000/1/messages/37025D32-CA82-4D86-9A0D-000000000000.wav",
    "message_type": "normal",
    "detail": null
  }
]
```

送信したメッセージが返ってきます。  

これで「メッセージを取得する」のチュートリアルを終わります。  
さらに詳細が知りたい場合は、[リファレンス(メッセージ一覧の取得)](/reference.html#get-roomsroomidmessages) をご覧ください。
