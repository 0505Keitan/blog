---
layout: post
title:  Googleformを半自動で送信する
date: 2019-12-12 00:00
permalink: auto-googleform-submit
tags: Tips Develop
---
授業開始時に送らないといけないフォームを半自動化した。

GoogleフォームはURL指定で事前に入力されてるものを代入することができる。

送るフォームはボタンとテキストエリアで構成されている。

<br>

今回は以下の画像を例にやって行く。

<img src="assets/images/2019-12-12.png" width="50%">

ボタンがあるところは

```
<div class="freebirdFormviewerViewNumberedItemContainer">
```

の中にある

```
<input type="hidden" name="entry.2071424204" jsname="L9xHkb" disabled="">
```

これを探して、中のnameにあるentryから始まるやつを使う。

テキストエリアは

```
<input type="text" class="quantumWizTextinputPaperinputInput exportInput" jsname="YPqjbf" autocomplete="off" tabindex="0" aria-label="テキストエリア" aria-describedby="i.desc.745931450 i.err.745931450" name="entry.2137555313" value="" dir="auto" data-initial-dir="auto" data-initial-value="">
```

のnameのやつを使う。

この例で行くとボタンは`entry.2071424204`、テキストエリアは`entry.2137555313`。

ボタンは`オプション１`、テキストは`hello`と入力し試しにURLにすると
```
https://docs.google.com/forms/d/e/....../viewform?usp=pp_url&entry.2071424204=%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3+1&entry.2137555313=hello
```
となる。なお、ボタンに関しては選択肢をURLエンコードする必要があるっぽい。

### 送信

送信ボタンはgetElementのclassname指定で`quantumWizButtonPaperbuttonFocusOverlay`と`exportOverlay`をclick()する。

```
document.getElementsByClassName('quantumWizButtonPaperbuttonFocusOverlay exportOverlay')[0].click()
```

もし、複数ページになる場合は`戻るボタン`が[0]で`次に進むボタン`が[1]になるっぽい。