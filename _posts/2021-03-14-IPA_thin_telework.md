---
layout: post
title:  シン・テレワークシステムの構成を特定してみる
date: 2021-03-14 00:00
permalink: 210312-IPA_thin_telework
tags: Blog
---

 - 2021/03/14 22:46
    - パーティションとラックと現用テープについてを追加
 - 2021/03/15 11:20
    - 結構重要なMicroSDカードが抜けていました。申し訳ありません。

IPAとNTT東がやっている「シン・テレワークシステム」のラズパイ周りの構成を~~特定~~探してみる。

**注・この記事でまとめているものはKeitanが勝手に考察したものです。何らかのことが起きても何も責任取りません。以上。**

特定材料は[「シン・テレワークシステム」 セキュリティ機能の大規模アップデートと実証実験の現状報告について](https://telework.cyber.ipa.go.jp/news/20200514/)のリリースとBSテレ東の[マゼランの羅針盤](https://www.tv-tokyo.co.jp/broad_bstvtokyo/program/detail/202102/23323_202102262100.html)とMBSの[情熱大陸](https://www.mbs.jp/jounetsu/2021/02_07.shtml)の３つ。

## ラック周り
> <img class="post-img" src="assets/images/img1FD.jpg">
>
> 引用元：[「シン・テレワークシステム」 セキュリティ機能の大規模アップデートと実証実験の現状報告について](https://telework.cyber.ipa.go.jp/news/20200514/) 図2

> <img class="post-img" src="assets/images/img200.jpg">
>
> 引用元：[「シン・テレワークシステム」 セキュリティ機能の大規模アップデートと実証実験の現状報告について](https://telework.cyber.ipa.go.jp/news/20200514/) 図3

まずこの２つの画像から~~特定~~探していく

### LANスイッチングハブ
図3を見る。右下にLANスイッチングハブが確認できる。機種名はケーブルで隠れていてあまり見えないが「08TPL V2」と文字が読める。

「08TPL V2」でググると一番上に[スイッチ｜CentreCOM GS908TPL V2 - アライドテレシス](https://www.allied-telesis.co.jp/products/list/switch/gs908tplv2/catalog.html)が出てくる。おそらくこれだろう。

### 電源ケーブル
電源ケーブルはUSB Type-AからType-Cであると確認できる。色は黒でケーブル本体には模様が確認できる。Amazonで「usb type c ケーブル」と検索するとそれらしいものが発見できた。図3のラズパイの電源周りを見ると同じようなロゴも確認できる。[Rampow USB Type C ケーブル](https://amzn.to/3ctE9fx)で正解だろう。

※ちなみにマゼランの羅針盤の序盤に登氏がAmazonで購入を行っているシーンがあるが、その場面でRAMPOWのパッケージが確認できる。

### 電源ハブ
電源ハブは見た目ぱっと見Anker製と思われる。真ん中にあるハブを見ると、左側が青く光っており、5ポートあるのが確認できる。これら情報を頼りにAmazonで検索する。[Anker PowerPort Speed 5](https://amzn.to/2Q2m1C5)が正解だと思われる。

### LANケーブル
LANケーブルはツメ保護ありで、コネクタ部分が白くなっているのでエレコム製だろう。太さ的にも[CAT6 ブルー LD-GPT/BU1/RS](https://amzn.to/38EsCcd)である可能性が高い。色違いで青と赤を利用している。ただし、長さは不明。

またマゼランの羅針盤のシーンにて左下に出てくるLANケーブルはビニールで覆われているのが確認できるため、簡易パッケージと予想できる。

なお、以下の画像の真ん中の白いLANケーブルを確認すると「CAT.6 UTP 30AWG CABLE 4PR MFD IN CHINA」と読める(気がする)ので全体でカテゴリ6のLANケーブルを使っているかもしれない？

> <img class="post-img" src="assets/images/img209.jpg">
>
> 引用元：[「シン・テレワークシステム」 セキュリティ機能の大規模アップデートと実証実験の現状報告について](https://telework.cyber.ipa.go.jp/news/20200514/) 図7

### ラック本体
流石にラック本体は類似品がありすぎて~~特定~~探すことができなかった。情報お待ちしてます。

### ラックのパーティション
Amazonで適当に`透明 ブックスタンド`で検索したらそれっぽいのが出てきた。多分これで正解。[セキセイ ブックエンド](https://amzn.to/3voX8Ri)

### 現用テープ
もはや検索で出てきた。特定する意味？？？[現用テープ](https://amzn.to/3lhaR8c)

## ラズパイ本体
### ラズパイ
ラズパイはテレビなどでも露出しているので簡単に確認ができる。Raspberry Pi 4である。メモリなどは確認できないが、先ほど書いたマゼランの羅針盤のシーンでAmazonの領収書に似せた紙が出てくる。ここには「【国内正規代理店品】Raspberry Pi4 ModelB 4GB ラズベリーパイ4 技適対応品【RS・OKdo版】」と書いてあるのでそのまま検索すると、[Amazon](https://amzn.to/3qPrZCW)で同じものが確認できた。(もしかすると、番組内で撮影のために用意しただけのものかもしれない)

### ヒートシンク
ヒートシンクに関しては先ほどのマゼランの羅針盤に出てきたシーンで左側にMHQJRHという文字が確認できる。検索すると、[Amazon](https://amzn.to/2NhKNgz)にて同様のヒートシンクが確認できた。(しかし、バカ高いぞ？？？)

### Micro SD
MicroSDに関しては、いろいろなものを使っているらしく、一つに縛ることはできなかった。

図4(右側)と図5(右のゴミ箱)にさらっとSanDiskのパッケージ(32GBモデル)らしきものと本体が見えているので、[サンディスク microSD 32GB UHS-I Class10](https://amzn.to/3cy2p06)を使っていることは確認できた。

# まとめ

| 製品            | Amazon                                             | 値段    |
|:---------------|----------------------------------------------------|--------|
|LANスイッチングハブ|[https://amzn.to/30EDSRH](https://amzn.to/30EDSRH)  |¥12,980 |
|電源ケーブル       |[https://amzn.to/3ctE9fx](https://amzn.to/3ctE9fx) |¥586    |
|電源ハブ         |[https://amzn.to/2Q2m1C5](https://amzn.to/2Q2m1C5)  |¥3,699  |
|LANケーブル(赤)   |[https://amzn.to/3lfbajC](https://amzn.to/3lfbajC)  |¥715   |
|LANケーブル(青)   |[https://amzn.to/38EsCcd](https://amzn.to/38EsCcd)  | ¥686   |
|ラズパイ4        |[https://amzn.to/3qPrZCW](https://amzn.to/3qPrZCW)  | ¥6,875 |
|ヒートシンク      |[https://amzn.to/2NhKNgz](https://amzn.to/2NhKNgz)  | ¥14,625|
|MicroSD         |[https://amzn.to/3cy2p06](https://amzn.to/3cy2p06)  |¥990    |
|(パーティション)    |[https://amzn.to/3voX8Ri](https://amzn.to/3voX8Ri)  | (¥731)   |
|(現用テープ)       |[https://amzn.to/3lhaR8c](https://amzn.to/3lhaR8c)  | (¥1,441~) |
|合計            |                                                    |¥41,156  |

[「シン・テレワークシステム」 セキュリティ機能の大規模アップデートと実証実験の現状報告について](https://telework.cyber.ipa.go.jp/news/20200514/)では`1 台あたりコストは、基板、ケース、ストレージ、電源、LAN ケーブルを合わせて 1.3 万円。`と書いてあるのに計算した結果¥41,156になってしまった。(パーティションと現用テープはのぞく)

おそらくマゼランの羅針盤にて`一部はもらったりしたもの`と登氏(役の俳優)は発言しているため、この値段だと考察できる。

## 参考
 - [IPA 「シン・テレワークシステム」 セキュリティ機能の大規模アップデートと実証実験の現状報告について](https://telework.cyber.ipa.go.jp/news/20200514/)
 - [BSテレ東 マゼランの羅針盤](https://www.tv-tokyo.co.jp/broad_bstvtokyo/program/detail/202102/23323_202102262100.html)
 - [MBS 情熱大陸](https://www.mbs.jp/jounetsu/2021/02_07.shtml)

※この記事にあるAmazonリンクは全てアソシエイトリンクを利用しています。