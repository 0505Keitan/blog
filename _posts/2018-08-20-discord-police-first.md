---
layout: post
title: Discord Policeができるまで【前編】
date: 2018-08-20 00:00
permalink: discord-police-first
tags: Qiitaからの移植
---
この記事はQiitaからの移植です。

## はじめに
結構前からDiscordBotを作りたいなぁと思っていてやっと一つ作れたので、できるまでの道のりを書いていく。
(※なお単語リストを別にする方法もありますが、今回は一つのファイルにまとめたいのでこの方法です。)

## 用意したもの
- MacBook Pro
- Discordのアカウント
- 自分が管理しているDiscordサーバー
- 集中力

# 1.下準備
### 1.2 node.jsのインストール
とりあえずDiscordのBOTを作る前に何の言語で作るかを考えた結果JavaScriptが自分には良さそうだったのでDiscord.jsを使うことにした。
まずはnode.jsのインストールから。
以下のコマンドでnode.jsのインストールをする

```shell
$ nodebrew install-binaly latest
```

### 1.3 DiscordのAPI TokenとClietIDを取得しておく
[DEVELOPER POTAL](https://discordapp.com/developers/applications/)で"Create an application"をクリック→"APP ICON"と"NAME"を決めて"Save Change"をクリック。
できたら画面の真ん中らへんにある"CLIENT ID"をコピー
次に左側にある"Bot"をクリック→"Add bot"→"Yes, do it!"をクリック。
その後、またまた画面真ん中らへんにある"TOKEN"の"Click to Reveal Token"をクリックしてTokenを取得。

# 2.とりあえずなんか作ってみる
### 2.1 以下の内容をコピペ

```js
const Discord = require('discord.js');
const client = new Discord.Client();

client.on('ready', () => {
  console.log(`Logged in as ${client.user.tag}!`);
});
 
 client.on('message', msg => {
  if (msg.content.match(/不適切な単語１/) || msg.content.match(/不適切な単語２/)) {
  	msg.delete(100);
    msg.reply('不適切な投稿を削除しました。');
  }
});

client.login('【YOUR TOKEN】');
```

# とりあえず実行
コピペが終わったら一回起動してみる。
起動は以下のコマンドで

```shell
$ npm install discord.js
$ node bot.js
```

とりあえずこれでBOT自体は動いているのでサーバーにBOTを招待。
以下の【ApplicationClientID】の場所に先ほどコピーしておいた"CLIENT ID"をペースト
`https://discordapp.com/oauth2/authorize?&client_id=【ApplicationClientID】&scope=bot&permissions=0`
そしてエンター。

うまくBOTがログインできていれば成功です！

ということで前編はここまで。暇があったらまた後半書きます。

