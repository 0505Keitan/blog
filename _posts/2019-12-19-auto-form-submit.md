---
layout: post
title:  自動でフォームを送信する
date: 2019-12-19 01:00
permalink: auto-form-submit
tags: Tips Develop
---
<a href="auto-googleform-submit">Googleformを半自動で送信する</a>の第二弾記事になるのでしょうか？

Chrome拡張機能を作って自動で送れるようにしました。

以下コード

```js
setInterval('a()',1000); //1秒毎にclock()を実行

function a() {
	if(document.getElementsByClassName('quantumWizButtonPaperbuttonFocusOverlay exportOverlay')[1] === undefined){
		document.getElementsByClassName('quantumWizButtonPaperbuttonFocusOverlay exportOverlay')[0].click();
	}else{
		document.getElementsByClassName('quantumWizButtonPaperbuttonFocusOverlay exportOverlay')[1].click();
	}
}
```

はい。ガバガバ実装ですね。

[1]の次へボタンが`undefined`だったら[0]をクリックするように仕向けただけです。

1秒に一回実行されて勝手にボタンが押されていきます。

ただしずっと付けっ放しにしておくと大事なフォームも飛ばしてしまうので普段は切っておくことにします。
