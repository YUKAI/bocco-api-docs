---
title: BOCCO API のアクセストークンを取得する
keywords: BOCCO API, リファレンス, BOCCO
last_updated: September 7, 2016
tags: [tutorial]
toc_at_sidebar : true
permalink: /get_access_token.html
folder: tutorial
summary: BOCCO API のアクセストークンを取得する方法を学びます。
---


## 準備

### APIキーの取得

アクセストークンを取得するためには、APIキーが必要です。まだお持ちでない場合は、[BOCCO APIの申し込み](index.html#bocco-api--1) を先に行ってください。

### ユーザ登録

BOCCO アプリから、予めユーザ登録を完了させてください。

## リクエスト

curl を使った場合のリクエスト例です。

- apikey : API申し込み時に送られてきたAPIキー。
- email: ユーザ登録時に入力したメールアドレス。
- password: ユーザ登録時に入力したパスワード。

```bash
curl -i "https://api.bocco.me/alpha/sessions" \
    -d "apikey=dqdsj4vu5w7f0mb1zmi1f7jpfhitpuplxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" \
    -d "email=user@example.com" \
    -d "password=strongpassword"
```

## レスポンス

### 成功

正しい情報が入力されていれば、 JSON で `access_token` が返ってきます。  
この `access_token` を使用して、その他のAPIを叩くことが出来ます。

レスポンスHeaders

```
HTTP/1.1 200 OK
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 08 Sep 2016 07:33:21 GMT
Content-Length: 129
Via: 1.1 vegur
```

レスポンスBody

```json
{
    "access_token" : "x2c4sjxacw4tq72q5h9vrsgjbcz3p55ixxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "uuid" : "e8bdc076-4045-45a7-abea-xxxxxxxxxxxx"
}
```

### 失敗

入力した情報のいずれか1つでも誤っていると、 `access_token` が返ってきません。

レスポンスHeaders

```
HTTP/1.1 401 Unauthorized
Server: Cowboy
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Vary: Accept-Encoding
Date: Thu, 08 Sep 2016 07:35:32 GMT
Content-Length: 42
Via: 1.1 vegur
```

レスポンスBody

```json
{
    "code" : 401001,
    "message" : "Invalid apikey"
}
```

これで「アクセストークンを取得する」のチュートリアルを終わります。  
さらに詳細が知りたい場合は、[リファレンス(アクセストークンの取得)](/reference.html#post-sessions) をご覧ください。


次の項目は、「[チャットルームを取得する](/get_joined_rooms.html)」です。

