---
layout: post
title:  ラズパイ4でエアコンを操作してみる
date: 2020-01-09 00:00
permalink: rpi4-aircon-onoff
tags: Develop
---
<style>
    .code {
        background: #272822;
        border-radius: 4px;
        padding: 10px 15px;
        color: #D9DCEF; }
        span .option {
            color: #D093E3; }
</style>

2020年最初の記事ですね。今年も色々作っていきます！

# 使用機材

 - [RaspberryPi4B (4GB)](https://www.amazon.co.jp/gp/product/B07YM3Z2QR)
 - [Pi 3 DIY Kit(22in1)](https://www.amazon.co.jp/gp/product/B01M6ZFNSS)
 - 赤外線LED - 503IRC2E-2AC

# pigpioの導入

まずはGPIOピンを制御するために`pigpio`を導入

```shell
$ sudo apt install pigpio python3-pigpio
```

デーモンを起動する

```shell
$ sudo systemctl enable pigpiod.service
$ sudo systemctl start pigpiod
```

# 赤外線学習用のコードをダウンロード

終わったら赤外線学習用のコードをダウンロードする。

```shell
$ curl http://abyz.me.uk/rpi/pigpio/code/irrp_py.zip | zcat > irrp.py
```

~~なぜか私の環境だとタイムアウトするのでMacでダウンロード、unzipしてラズパイにscpした。~~

失敗した理由がわかりました。IP固定するために色々いじってた結果DNSサーバーの指定をしていなかった?っぽかったです。
多分このままでもうまくいきます。

# 配線

#### 全景
<img class="post-img" src="assets/images/2020-01-09-01.jpg">

#### ブレッドボード
<img class="post-img" src="assets/images/2020-01-09-02.jpg">

配線細かく書きたいんですけどめんどくさいので割愛。写真見てなんとかしてください()

# 赤外線学習の準備

これで17ピンを出力に、18ピンを入力に設定する。

```shell
$ echo 'm 17 w   w 17 0   m 18 r   pud 18 u' > /dev/pigpio
```

# 信号記録

```shell
$ python3 irrp.py -r -g18 -f codes air_con:on --no-confirm --post 50
```

|オプション|意味|
|:-:|:-:|
|-r|受信した信号を記録するモード|
|-g18|GPIO18を使って信号を受信する|
|-f codes|受信した信号を出力するファイル名を指定する|
|--no-confirm|確認のために同じ信号を2回要求しない|
|--post 50|50ms無信号の場合に信号が終わったと判断する|
|air_con:on|信号の識別子。わかりやすい名前をつける|

参照元 : <a href="https://qiita.com/gorohash/items/598d69a63bd6b4308291">エアコンのリモコン信号を解析する ~デコード編~ - Qiita</a>

# 信号発信

このコードだけネット上に落ちていたので参照先がなく、曖昧です。

```shell
$ python3 irrp.py -p -g17 -f codes air_con:on
```

|オプション|意味|
|:-:|:-:|
|-p|送信モード？|
|-g17|GPIO17を使って信号を発信する？|
|-f codes|発信するファイル名を指定する|
|air_con:on|信号の識別子。さっきのやつ|

 - <a href="https://jorublog.site/electronic-work-smart-remocon-program/">大学生の電子工作　スマートリモコン(プログラム) - じょるブログ</a>
 - <a href="https://qiita.com/gorohash/items/598d69a63bd6b4308291">エアコンのリモコン信号を解析する ~デコード編~ - Qiita</a>
 - <a href="https://qiita.com/takjg/items/e6b8af53421be54b62c9">格安スマートリモコンの作り方 - Qiita</a>