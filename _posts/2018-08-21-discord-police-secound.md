---
layout: post
title: Discord Policeができるまで【後編】
date: 2018-08-21 00:00
permalink: discord-police-secound
tags: Qiitaからの移植
---
この記事はQiitaからの移植です。

前回はBOTをサーバーに招待するまでをやったので、後編はBOTを完成させる。
[前回の記事-Discord Policeができるまで【前編】](https://0505Keitan.com/blog/discord-police-secound)

# 4.権限の追加
前回のままだとBOTがメッセージを消去できないのでメッセージ管理権限を与える。

## 役職を作る
1.Discordの左上のメニューよりサーバー設定をクリックする
2.左側のメニューの`役職`をクリックする
3.真ん中らへんにある`@everyone`の右上にある+ボタンを押して、新しい役職を追加する
4.`new role`をクリックし、役職名を好きなものにする(例えばBOTなど)
5.役職の設定の下の方にある`メッセージの管理`をオンにする
6.下に出てきたセーブボタンを押して変更をセーブする

## BOTに役職を適用
1.先ほどの左側のメニューの下補の方にあるユーザー管理のメンバーをクリックする
2.招待したBOTの右側にある+ボタンから先ほど追加した役職を追加する
3.ESCキーを押してチャットに戻る。

これでメッセージを消せるようになりました。
# 5.完成
一応前編で起動してあるターミナルのコマンドを`Ctrl+c`で一旦停止させて、もう一度以下のコマンドを実行。

```shell
$ node bot.js
```

ですが、このままだとPC上のターミナルを起動していないと動かないのでどっかのVPSか友人にサーバーを借りる。僕は幸いにも友人がサーバーを貸してくれたのでそれを使う。一応バックアップ用にConoHaのVPSも借りた。
# 6.サーバー上で実行する
ここではConoHaのVPSを使って説明する。
1.まずConoHaのサイトで会員登録をする。[ConoHa](https://www.conoha.jp/)
2.必要事項を記入し、続行
3.お支払い情報を入力。ここではプリペイドカードはチャージにしか使えない。
4.サーバーの構成を選ぶ。僕は512MBでUbuntuを選んだ。ここから先はUbuntuでのやり方を書く

とりあえずこれで一通りの準備は完了。次にファイルをアップロードしたりサーバー上で動かす設定をしていく。
## SSHでConoHaのサーバーに接続。
(※この前に色々rootとかの設定があるのですが、今回は割愛。詳しくはConoHaの公式サイトに載っています。)
ということでサーバーの準備はできたので早速サーバーの設定をしていく。
まずはsshでConoHaのサーバーにつなぐ
1.コントロールパネルの左側の`サーバー`をクリック
2.出てきた画面の真ん中らへんにあるコンソールをクリック
こうするとブラウザにコンソールが出てくるのでこれを使う
3.コンソールの`login:`のところに`root`と入力し続いてrootパスワードを入力しエンター
4.これでサーバーにログインできたので、サーバーでBOTを動かすために`node.js`と`discord.js`とforeverをインストール。
# サーバー上に色々インストール
## node.jsインストール
ではまず最初に`node.js`からいれていく。
ここからはコンソールの画面オンリーになるので上から順番に一つづつ実行してください。

```shell
$ sudo apt-get install -y nodejs npm
$ sudo npm cache clean
$ sudo npm install n -g
$ sudo n stable
$ sudo ln -sf /usr/local/bin/node /usr/bin/node
$ sudo apt-get purge -y nodejs npm
```

これで導入自体は完了しているので最新バージョンか確認

```shell
$ node -v
v10.8.0
```

これで`node.js`は導入完了
## foreverインストール
次にBOTを常時動かすために`forever`というずっと動作し続けるやつを導入。

```shell
$ sudo npm install -g forever
``` 
はい終了。これでインストールは終わったので次にファイルのアップロードをしていく。
# ファイルアップロード
ファイルアップロードはコンソールでもできるが、僕はFTPマネージャーを使った。
ではFTPマネージャーで接続をする。なおFTPマネージャーは使えれば多分なんでも可
(！注意！今回はrootで接続するため間違ってファイルを消すことがないように十分注意してください)
1.FTPマネージャーを起動し、新規接続
2.サーバーの欄にConoHaのコントロールパネルにあるIPアドレスを入力
3.ユーザ名を`root`。パスワードはルートパスワードを使う
4.接続。
接続できたらファイルがいっぱいあると思うのでメインフォルダに移動し適当なフォルダを作る
作れたらその中に作ったBOTのファイルをぶち込む

これでアップロードも終了。
# いよいよ起動
これであとはサーバー上で起動するだけになった。
それでは起動していく。
1.ConoHaに戻り、コンソールを開く
2.ログインされてなかったらログインし直す
3.さっきアップロードした場所へcdで移動
4.移動できたらそのフォルダで以下のコマンドを打つ

```shell
$ npm install discord.js
```
これで`discord.js`もインストールできたのでいよいよ実行する。

コンソールで次のコマンドを打って実行

```shell
$ forever start bot.js
```

これで全てが終了。
もう一度Discordサーバーに戻ってきちんと動いているか確認して、動いていたら完成！

# おまけ
一応foreverを止めたりする時用のコマンドも紹介しておく
・動いているBOTリストを表示させる

```shell
$ forever list
```

・BOTを止める

```shell
$ forever stop 【uid】
```

なお`uid`はリストから参照可能
