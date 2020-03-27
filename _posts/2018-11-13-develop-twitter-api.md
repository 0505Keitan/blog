---
layout: post
title: TwitterAPIをいじってみた
date: 2018-11-13 00:00
permalink: develop-twitter-api
tags: Qiitaからの移植
---
この記事はQiitaからの移植です。

# 今回のミッション
昔設定していたWelcomeメッセージを削除すること

# １、TwitterのDeveloperアカウントを取得
詳しいことは[この記事](https://qiita.com/tdkn/items/521686c240b0c5bc6207)に書いてあるので割愛
# ２、Twitter公式のDocumentsを漁る
今回はDMのWelcomeメッセージを削除したかったので[これ](https://developer.twitter.com/en/docs/direct-messages/welcome-messages/overview)を読んだ
# ３、twurlをインスコ
どうやらtwurlが必要らしいのでインスコ

```shell
$ gem install twurl
```
DeveloperポータルからConsumerKeyとConsumerSecretを回収して代入

```shell
$ twurl authorize --consumer-key {CONSUMER-KEY}       \
                --consumer-secret {CONSUMER-SECRET}
```
# ４、DMのリストを取得

```shell
$ twurl -X GET "/1.1/direct_messages/welcome_messages/list.json?count=2"
```
この時にcount=2を取得したいメッセージの数を入れる

```json
{
  "welcome_messages": [
    {
      "id": "844385345234",
      "created_timestamp": "1470182274821",
      "message_data": {
        "text": "Welcome!",
        "attachment": {
          "type": "media",
          "media": {
            ...
          }
        }
      }
    },
    {
      "id": "844385345238",
      "created_timestamp": "1470182275399",
      "message_data": {
        "text": "Welcome Again!",
        "attachment": {
          "type": "media",
          "media": {
            ...
          }
        }
      }
    }
  ],
  "next_cursor": "NDUzNDUzNDY3Nzc3"
}
```
こんな感じで降ってくるので消したいメッセージのidを調べる
idをゲットしたら

```shell
$ twurl -X DELETE /1.1/direct_messages/welcome_messages/destroy.json?id={さっきのID}
```
これに代入して終了

