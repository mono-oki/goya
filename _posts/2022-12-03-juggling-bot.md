---
title         : "ジャグリングクラブ・練習会を探すLINEbotを作りました"
---

## はじめに

こんにちは。Imagawayakiと申します。  
この記事は、[ジャグリング Advent Calendar 2022](https://adventar.org/calendars/7345){:target="_blank"} の記事として投稿しました。  
アドベントカレンダーの3番手になります。よろしくお願いします。  

---

最近寒いですね。  
普段屋外でジャグリングを練習していても、寒さと乾燥のおかげで「室内で練習したい！」と思ってしまいます。  
そこで、近くのジャグリングクラブ・練習会を探してくれるLINEbotを作ってみました。  
実はあなたの近くにもジャグリングクラブがあるかもしれません。ぜひ遊んでみてください。  

## 使い方

①こちらのQRコードからLINEbotを友だち登録してください。  
![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-000.png)  

②トーク画面に入り、画面左下の「＋」マークをタップしてください。  
![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-001.jpeg)  

③出てきたメニューから「位置情報」を選択してください。  
![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-002.jpeg)  

④マップから位置情報を設定して「送信」をタップしてください。  
![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-003.jpeg)  

⑤近くのジャグリングクラブや練習会の情報をbotが答えてくれます。  
※近くの3件を返します。  
![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-004.jpeg)  

## 仕組み

LINEbotはMessaging APIとGoogle Apps Scriptで作られたbotサーバーで動いています。  
ユーザーがトーク画面から位置情報を送信すると、Webhookによりbotサーバーに位置情報データが送られて近くのジャグリングクラブを検索します。  

大まかな仕組みはこんな感じですね。  
![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-005.png)  

検索対象となるジャグリングクラブの情報はGoogleスプレッドシートから取得します。  
スプレッドシートには「クラブ名」「練習場所」「WebサイトURL」「緯度／経度情報」が含まれています。  

※このスプレッドシートはインターネットなどから集めた情報をもとに個人で作成しました。  
「情報が古い！」「間違いがあるぞ」などといった点がありましたらTwitter等でご連絡いただけますと助かります、、  

[スプレッドシートの内容(CSV形式)]({{site.baseurl}}/assets/jugglingClubList.csv)  

## おわりに

海上で遭難しても練習場所を探せます。  

![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-006.JPG)  

![]({{site.baseurl}}/assets/img/posts_image/2022-12-03-001/2022-12-03-007.JPG)  

これで安心ですね。  

終り  
