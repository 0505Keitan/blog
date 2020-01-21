---
layout: post
title:  画像をresponsive対応にした
date: 2019-12-12 00:00
update: 2019-12-16 00:00
permalink: image-responsive-design
tags: Develop Blog Design 進捗
---
cssにこのコードを足しただけ。

まあ普通に`@media`で500px以下のwidthを100%にしてるだけですね。

レスポンシブデザイン対応難しいなぁ.....。
```css
.post-img {
    width: 50%;
}

@media screen and (max-width: 500px) {
    .post-img {
        width: 100%;
    }
}
```

以下サンプル画像。PCで見るときは50%、スマホなどwidthが500px以下の場合はフルで表示されるはず。

<img class="post-img" src="assets/images/2019-12-16.png">