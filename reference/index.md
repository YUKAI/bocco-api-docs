---
title: BOCCO API リファレンス
keywords: BOCCO API, リファレンス, BOCCO
last_updated: September 6, 2016
tags: [reference]
sidebar: reference
permalink: /reference
folder: reference
---

## 共通仕様
### API Endpoint

https://api.bocco.me/alpha

### 文字コード

UTF-8

### レスポンスの Content-Type

`application/json`


## アクセストークンの取得 [POST /sessions]

ユーザの入力したアカウント情報からセッション情報を作成し、アクセストークンを取得します。

### リクエスト

#### Content-Type

`application/x-www-form-urlencoded`

#### Body

+ apikey (必須, string, `xxxxxxxxxxx`) ... BOCCOサポートから受け取ったAPIキー。
+ email (必須, string, `email@example.com`) ... ユーザ登録時に入力したメールアドレス。
+ password (必須, string, `xxxxxxxxxxx`) ... ユーザ登録時に入力したパスワード。

### レスポンス

#### 200

新しい `access_token` が返されます。

```json
{
    "access_token" : "f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6",
    "uuid" : "e8bdc076-4045-45a7-abea-b4a34e00a417"
}
```

#### 401
    
`apikey`, `email`, `password` のいずれかが誤っている場合 401 が返ります。

```json
{
    "code": 401001,
    "message" : "Unauthorized"
}
```

## 入っているチャットルームの取得 [GET /rooms/joined]

### リクエスト

#### クエリストリング

+ access_token (必須, string, `f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6`) ... 取得したアクセストークン。

### レスポンス

#### 200

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

## メッセージ一覧の取得 [GET /rooms/{room_id}/messages]

このAPIにアクセスするためには、追加の権限が必要です。BOCCOサポートにお問い合わせください。

### リクエスト

#### URLパラメータ

+ room_id (必須, string, `2cf4c5b5-8991-46d7-a373-246a9998ab45`) ... チャットルームのuuid。

#### クエリストリング

+ newer_than (任意, integer, `1`) ... 指定した `id` より新しいメッセージが返ります。指定した `id` のメッセージは含まれません。
+ older_than (任意, integer, `9`) ... 指定した `id` より古いメッセージが返ります。指定した `id` のメッセージは含まれません。
+ access_token (必須, string, `f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6`) ... 取得したアクセストークン。
+ read (任意, bool, `1`) ... 1 が指定された場合、レスポンスに含まれる最新のメッセージを既読にします。


### レスポンス

#### 200

並び順はメッセージは `id` の *昇順* です。アプリのメッセージ一覧と同じ並び順です。  
`detail` キーの内容は、 `message_type` によって異なります。

```json
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
```

##### message_type が `system.human_joined` もしくは `system.sensor_joined` の場合

```json
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
```


## メッセージの送信 [POST /rooms/{room_id}/messages]

### リクエスト

#### Content-Type

`application/x-www-form-urlencoded`, `multipart/form-data`

#### Body

+ text (任意, string, `Hi!`) ... `text` を送信する場合、 `media` パラメータを `text` に指定します。
+ audio (任意, binary) ... audioファイルをバイナリで指定します。 `audio` を送信する場合、 ヘッダーに `Content-Type: multipart/form-data` を指定して、`media` パラメータを `audio` に指定します。
+ image (任意, binary) ... imageファイルをバイナリで指定します。 `image` を送信する場合、 ヘッダーに `Content-Type: multipart/form-data` を指定して、`media` パラメータを `image` に指定します。
+ unique_id (必須, string, `F7827189-E419-4012-820F-4AFD608E1ED2`) ... 冪等性を担保するため、クライアント側で生成したユニークIDを指定します。
+ media (必須, string, `text` or `audio` or `image`) ... 送信するメッセージの種類を指定します。
+ access_token (必須, string, `f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6`) ... 取得したアクセストークン。

##### リクエストBody例 (multipart/form-data; boundary=BOUNDARY)

```
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
```


### レスポンス

#### 201

メッセージが作成された。

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

#### 200

メッセージが既に保存されたものであった場合、ステータスコード 200 が返ります。 unique_id によって識別されます。

```json
{
  "date":"2014-12-23T14:58:06+09:00",
  "id":60,
  "media":"text",
  "message_type":"normal"
  "unique_id":"4ECD2AC5-C9EB-4415-BFAF-BB212F89003A"
}
```

## イベント取得 (ストリーミング) [GET /rooms/{room_id}/subscribe]

このAPIにアクセスするためには、追加の権限が必要です。BOCCOサポートにお問い合わせください。

### リクエスト

#### URLパラメータ

+ room_id (必須, string, `2cf4c5b5-8991-46d7-a373-246a9998ab45`) ... チャットルームのUUID。

#### クエリストリング

+ newer_than (必須, integer, `1`) ... 指定した `id` より新しいメッセージが返ります。指定した `id` のメッセージは含まれません。
+ access_token (必須, string, `f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6`) ... 取得したアクセストークン。
+ read (任意, bool, `1`) ... 1 が指定された場合、レスポンスに含まれる最新のメッセージを既読にします。

### レスポンス

並び順はメッセージは `id` の *昇順* です。アプリのメッセージ一覧と同じ並び順です。

このAPIはロングポーリングのリクエストです。 `newer_than` パラメータより新しいメッセージが来た場合に、レスポンスとして返ります。来なかった場合はタイムアウトとなります。

レスポンスには `event` として `message` または `member` が返ります。全てのイベントには `event` と `body` キーが含まれます。クライアントは `event` キーの中身を見て、処理を分岐する必要があります。 `message` イベントのフォーマットは、 "メッセージの取得" API のフォーマットと同じです。



#### 200

##### event が `message` の場合

```json
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
```

##### event が `member` の場合

```json
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
```

#### 408
 
ロングポーリング中に、 `newer_than` より新しいメッセージが届かなかったら、 408 が返ります。

```
{
    "code" : 408001,
    "message" : "Request Timeout"
}
```

## メッセージを既読にする [POST /rooms/{room_id}/messages/{message_id}/read]

このAPIにアクセスするためには、追加の権限が必要です。BOCCOサポートにお問い合わせください。

### リクエスト

#### URLパラメータ
    
+ room_id (必須, string, `2cf4c5b5-8991-46d7-a373-246a9998ab45`) ... チャットルームのUUID。
+ message_id (必須, int, `123`) ... メッセージの `id`。

#### クエリストリング

+ access_token (必須, string, `f2a87cb04ef23a714a1a438f567abb6812f8fbd9a33e28b789202e45190739d6`) ... 取得したアクセストークン。

### レスポンス

#### 200

```json
{}
```


## エラー

### HTTP ステータスコード

HTTP ステータスコードは本来の意味で使われます。詳しくは、[HTTPステータスコード(ウィキペディア)](https://ja.wikipedia.org/wiki/HTTP%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%89) をご覧ください。

+ 401 : `access_token` が不正です。全てを初期化して、初めの手順からやり直す必要があります。
+ 400 : GET/POST リクエストのパラメータが不正です。パラメータを確認してください。
+ 404 : リクエストされたリソースが見つからない、もしくは権限がありません。リソースがアクセス可能なことを確認してください。
+ 408 : リクエストタイムアウト。ネットワーク状態を確認してください。
+ 429 : 過剰なリクエスト頻度。しばらく待ってから、再度リクエストしてください。
+ 500 : 内部エラー。BOCCOサポートへお問い合わせください。
+ 502 : 不正なゲートウェイ。BOCCOサポートへお問い合わせください。
+ 503 : サービス利用不可。BOCCOサポートへお問い合わせください。

### レスポンス

HTTPステータスが 200 以外のとき、下記のような Body が返ります。

```json
{
    "code" : xxxxxx,
    "message" : xxxxxxxx
}
```

`code` はエラー識別子です。各APIの詳細を確認してください。  
`message` はローカライズされた人間可読なエラーメッセージです。


{% include links.html %}