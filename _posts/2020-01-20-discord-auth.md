---
layout: post
title:  ナポアンDiscordサーバーの荒らし対策について
date: 2020-01-20 00:00
permalink: discord-auth
tags: Develop
---
マイクラ界で意外と有名な<a href="https://napoan.com">ナポアン</a>氏はDiscordサーバーをやっていて、そのサーバーの技術担当してるんですけど、そこで結構前からやってる荒らし対策について書きます。多分。

 - ちなみにこのアイデアは<a href="https://twitter.com/stmkza">stmkza</a>氏に教えていただきました。
 - スクショでやばそうなやつは削除してるんでよろ

このサーバーではjoinするとまず認証サイトで認証しないとチャンネルなどで話すことができません。

# サーバーにjoin後
新規のみ閲覧可能なチャンネルにて認証サイトに飛ぶように促します。
<img src="assets/images/2020-01-20-01.png">
<br><br>

# 認証サイト
認証サイトに飛んだら、GoogleのreCapcha認証をしなければいけません。
<img src="assets/images/2020-01-20-02.png">
<br><br>

# 終わり
Discordのサイトでログインしてもらいcallback情報をもとにuser役職をつければ終了です。
<img src="assets/images/2020-01-20-03.png">
<br><br>

ナポアンDiscordサーバーは <a href="https://discord.gg/GUSKXZZ">こちら</a> からどうぞ。あまりオススメしませんけど(

書いてる途中に公開したくなくなったんですけどとりあえずあげます。知らない間に削除されてるかもしれません。


 - <a href="https://discord.gg/GUSKXZZ">NAPOAN SERVER - Discord</a>
 - <a href="https://discord.gg/gNkUbpb">PROGLOVE - Discord</a>