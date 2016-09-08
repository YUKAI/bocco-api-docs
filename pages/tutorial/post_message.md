---
title: メッセージを送信する
keywords: BOCCO API, リファレンス, BOCCO
last_updated: September 7, 2016
tags: [tutorial]
toc_at_sidebar: true
permalink: /post_message
folder: tutorial
summary: BOCCO API を使って、メッセージを送信する方法を学びます。
---

## 準備

### アクセストークンの取得

API リクエストに、 `access_token` が必要です。   
[アクセストークンの取得](get_access_token) の手順に従い、`access_token` を事前に取得してください。

### room_id の取得

メッセージを送信するチャットルームを指定するために、 `room_id` が必要です。
[チャットルームの取得](get_joined_rooms) の手順に従い、`uuid` (以後 `room_id` として使う) を事前に取得してください。


## リクエスト

curl を使った場合のリクエスト例です。  
ここでは、「こんにちはBOCCO！」というテキストメッセージを送ります。

`room_id` として、チャットルームの取得で返ってきた `E7607BA3-2AA0-4DEB-8959-E384B3DE82B0` を使用します。

リクエストBody

- access_token : 取得したアクセストークン。
- unique_id : 任意のユニークidを指定します。ユニークidが同じメッセージは送られません。[UUID version 4](https://ja.wikipedia.org/wiki/UUID#.E3.83.90.E3.83.BC.E3.82.B8.E3.83.A7.E3.83.B34) の使用が推奨です。ここでは、 `uuidgen` コマンドを使用しています。
- text: 送りたいメッセージを指定します。ここでは `こんにちはBOCCO！` を指定します。
- media: テキストメッセージを送信する場合は、 `text` を指定します。

リクエストHeaders

- Accept-Language: `ja` として日本語を指定します。


```bash
curl -i "https://api.bocco.me/alpha/rooms/E7607BA3-2AA0-4DEB-8959-E384B3DE82B0/messages" \
    -d "access_token= f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6"
    -d "unique_id=`uuidgen`"
    -d "media=text"
    -d "text=こんにちはBOCCO！"
    -H "Accept-Language: ja"
```


## レスポンス

### 成功

正しい情報で送信すれば、 JSON でメッセージのオブジェクトが返ってきます。  

レスポンスHeaders

```
HTTP/1.1 201 Created
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 08 Sep 2016 08:59:20 GMT
Content-Length: 565
Via: 1.1 vegur
```

レスポンスBody

```json
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
```

### 失敗

何らかの情報が誤っていると、 401 が返ります。

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

これで「メッセージを送信する」のチュートリアルを終わります。  
さらに詳細が知りたい場合は、[リファレンス(メッセージの送信)](/reference#post-roomsroomidmessages) をご覧ください。
 

次の項目は、「[メッセージを取得する](get_messages)」です。
