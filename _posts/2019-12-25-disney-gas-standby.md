---
layout: post
title: ディズニーリゾートの待ち時間をGASを使ってLINEで見れるようにした話
date: 2019-12-25 00:00
permalink: disney-gas-standby
tags: Qiitaからの移植
---
この記事は[N高等学校 Advent Calendar 2019](https://qiita.com/advent-calendar/2019/n-highschool)の最終日です。

こんにちは。N高等学校2年の0505Keitanと申します。
クリスマスなので今回はディズニーの待ち時間をLINEで見れるようにしたことでも書こうかと思います。

他にも色々作ってるんですけどいい感じに記事にできるやつがなかったのでこれにしました(
ちなみに紹介記事なので作ってみよう系じゃないです。

# 作ったもの
以下の画像のように待ち時間を垂れ流します。
ちなみに公開はしないです。結構ガバガバ実装なので...。
なお、他にもショーの時間とかも見れるようになってます。詳しくは[コチラ](https://scrapbox.io/0505Keitan/TokyoDisneyResort_Unofficial_LINE_Account)。
<img width="358" alt="image.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/179129/08d49cad-5d90-e990-7118-def1c7bd3758.png">

データ取得先なんですけど、ディズニーは待ち時間APIを出していない(当たり前)なので、[tokyodisneyresort.info](https://tokyodisneyresort.info)の情報を使わせていただくことにしました。このサイトに10分に一回(7分間隔)アクセスし、スクレイピングして更新、表示させます。

念のためサーバーに過負荷を掛けさせないため、要求ごとに取得させるのではなく、取得したデータを一度スプレッドシートに保存し、メッセージごとに呼び出しています。

# 使用技術
- Google Apps Script
- Google Spreadsheet
- Parser
- Moment

実行フローとしては
① [tokyodisneyresort.info](https://tokyodisneyresort.info)にアクセス
② 取得データを整形して、Googleスプレッドシートに保存
③ LINEからWebhookでリクエストが飛んでくる
④ スプレッドシートに情報を取りに行く
⑤ データをGASに渡す
⑥ GASからLINEに送信する
という順番です。

![qiita.001.jpeg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/179129/eb22c48b-f5ec-06ab-f7ac-e7b5308a93cd.jpeg)

# GASのライブラリ
今回は取得したhtmlをパースしてくれる`Parser`と日付の処理を簡単にしてくれる`moment`を利用しています。

導入はGASの編集画面から`リソース`→`ライブラリ`→Add a libraryの欄に以下のプロジェクトキーを入力して追加をクリック！

- Parser
    - `M1lugvAXKKtUxn_vdAG9JZleS6DrsjUUV`
- Moment.js
    - `MHMchiX6c1bwSqGM1PZiW_PxhMjh3Sh48`

# コード
結構長くて処理複雑なので主要部分だけ書きます。

#### ランドとシーのスタンバイ時間を配列で出す。
```js
var response = UrlFetchApp.fetch('https://tokyodisneyresort.info/realtime.php?park=land&order=name', {'validateHttpsCertificates' : false});
var response_c = UrlFetchApp.fetch('https://tokyodisneyresort.info/realtime.php?park=sea&order=name', {'validateHttpsCertificates' : false});
var html = response.getContentText('UTF-8');
var html_c = response_c.getContentText('UTF-8');
  
var data = Parser.data(html).from('<div class="attr_name">').to('</div>').iterate();
var data_c = Parser.data(html_c).from('<div class="attr_name">').to('</div>').iterate();
var fdata = Parser.data(html).from('<div class="attr_wait">').to('</div>').iterate();
var fdata_c = Parser.data(html_c).from('<div class="attr_wait">').to('</div>').iterate();
  
for(i=0;i<data.length;i++){
  data[i]=data[i].replace(/	/g,'').replace('&park=land">','').replace('</a>','').replace('<span class="new">NEW</span>','').replace('amp;','').replace(/\n/g,'').replace(/ /g,'');
  fdata[i]=fdata[i].replace(/<span class="fp">【FP：(TICKETING|TICKETING_END|NOT_TICKETING_TODAY)】<\/span>/g,'（FP対象）').replace(/<span class="greeting_timetable">/g, '').replace(/<\/span>/g, '').replace(/ /g,'').replace(/\n/g,'').replace(/<br\/>/g,'\n').replace(/分/g,'分').replace(/	/g, '').replace(/^$/g, '情報なし');
  data[i]='【'+data[i]+'】\n'+fdata[i]+'\n\n';
}
for(i=0;i<data_c.length;i++){
  data_c[i]=data_c[i].replace(/	/g,'').replace('&park=sea">','').replace('</a>','').replace('<span class="new">NEW</span>','').replace('amp;','').replace(/\n/g,'').replace(/ /g,'');
  fdata_c[i]=fdata_c[i].replace(/<span class="fp">【FP：(TICKETING|TICKETING_END|NOT_TICKETING_TODAY)】<\/span>/g,'（FP対象）').replace(/<span class="greeting_timetable">/g, '').replace(/<\/span>/g, '').replace(/ /g,'').replace(/\n/g,'').replace(/<br\/>/g,'\n').replace(/分/g,'分').replace(/	/g, '').replace(/^$/g, '情報なし');
  data_c[i]='【'+data_c[i]+'】\n'+fdata_c[i]+'\n\n';
}
```

#### LINEへ送信
```js
function reply(message, token) {
  UrlFetchApp.fetch(line_endpoint, {
      'headers': {
        'Content-Type': 'application/json; charset=UTF-8',
        'Authorization': 'Bearer ' + CHANNEL_ACCESS_TOKEN,
      },
      'method': 'post',
      'payload': JSON.stringify({
        'replyToken': token,
        'messages': [{
          'type': 'text',
          'text': message,
        }],
      }),
    });
  return;
}
```
LINEでレスポンスをするには`doPost`に含まれている`replyToken`を指定しなければなりません。


# 問題点と改善結果
#### ①22:00以降は結果が同じになる
実行しても同じ結果なので22:00以降は各処理前に時間を取得してreturnさせました。(もっといい方法があるはず)

```js
if(Moment.moment().format('HH')>=22||Moment.moment().format('HH')<08){
    sheet_standby.getRange('A1').setValue('東京ディズニーリゾートは現在閉園中です。');
    sheet_standby.getRange('B1').setValue('東京ディズニーリゾートは現在閉園中です。');
    sheet_standby.getRange('A2:B2').setValue('最終取得時間：'+Moment.moment().format('HH:mm'));
    return;
  }
```

#### ②GASでアクセスするときSSLエラーが出る
(あまりいい方法ではないが)
無理やりアクセスさせるために`UrlFetchApp`に`{'validateHttpsCertificates' : false}`をつけました。

```js
UrlFetchApp.fetch('https://tokyodisneyresort.info/realtime.php?park=land&order=name', {'validateHttpsCertificates' : false});
```

# まとめ
やっぱりGASはいいですよね(あとディズニー)！！！
ぜひ公式API出して欲しいです。
実装もすぐに終わるのでGASはぜひ使いましょう！！！本当に便利です。

ガバガバ実装なの許してください~~なんでもしますから~~

