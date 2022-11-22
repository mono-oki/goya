---
title         : "Twitterでいいねした動画を保存しよう！"
---

## はじめに

こんにちは。  
この記事は、[ジャグリング Advent Calendar 2021](https://adventar.org/calendars/6305){:target="_blank"} の記事として投稿しました。  

---

皆さんはTwitterでジャグリング動画を観ていますか？私も日々素晴らしいジャグリングの技をTwitterで眺めています。  

気に入った動画は後で見返すために「いいね」しているのですが、Twitter公式クライアントには動画保存機能がないので「いいね」した動画はネット環境が無いと視聴できません。  

かつて携○動画変換君で変換した3GP形式の動画をガラケーに入れて感動し、ジャグリング練習の時に動画をiPodに入れていたタイプの人間なので、TwitterAPIを直接叩いて過去にいいねした動画を保存するプログラムを書いてみました。   
準備したことや作ったものをまとめて記事にしてみたので、よろしければ何かの参考にしてもらえると嬉しいです。  

※保存した動画は個人で楽しむ範囲にしましょう。  

### ①Twitter Delevoperに登録
TwitterAPIを利用するために、Twitterの公式サイトからDeveloper登録を行いました。  
[Twitter Developer Platform](https://developer.twitter.com/){:target="_blank"}

ちなみに、アカウントに電話番号が登録されていないと先に進めません。ナンテコッタイ  
![]({{site.baseurl}}/assets/img/posts_image/2021-12-10-001/2021-12-10-001.png)

* Developerへの登録手順はこちらのサイトを参考にしました。APIの利用目的は英語で打ち込むので、中学英語力を駆使して突破しました。
  * [【API】TwitterAPIの登録方法！アカウント申請方法と認証されるまでの手順をレポートします！【2021年1月版】](https://correct-log.com/how_to_get_twitter_api/){:target="_blank"}
  * [【Twitter API v2.0】利用申請通ったので方法を記録](https://qiita.com/ume-san/items/c32e5378e66cd888758c){:target="_blank"}
* 登録が終わるとAPIを利用するのに必要な認証情報4点セット「API key」「API secret key」「Access token」「Access token secret」が発行されます。
  * この4点セットは絶対に他の人に漏れないよう厳重に管理する必要があります。
  * 登録完了後こんな画面が表示されるので、記載されている情報をすぐにメモしておきました。  
![]({{site.baseurl}}/assets/img/posts_image/2021-12-10-001/2021-12-10-002.png)

### ②Node.jsのインストール
小学生の頃マウスポインタを画像が追いかける系のホームページが大好きだった人間なので、JavaScriptで書くことにしました。 

* JavaScriptを動かすエンジンとして「Node.js」を使います。公式ページからインストーラをダウンロードできます。
[Node.js](https://nodejs.org/ja/){:target="_blank"}


* コマンドプロンプト（Macの場合はターミナル）で下記コマンドを打ち、バージョンが表示されてエラーが出なければ大丈夫です。
```
$ node -v
v12.22.3
$ npm -v
8.1.4
```
※↑は筆者の環境です。

### ③コードを書く
書きました。動けばいいやの精神なので、書きっぷりはご容赦のほどを、、
```javascript
const fs = require('fs');
const rp = require('request-promise');
const Twitter = require('twitter');

// ツイート情報の配列
const content_arr = [];
// 認証情報
const client = new Twitter({
  consumer_key: '<API keyを入れる>',
  consumer_secret: '<API secret keyを入れる>',
  access_token_key: '<Access tokenを入れる>',
  access_token_secret: '<Access token secretを入れる>'
});
// リクエストパラメータ
const params = {
  screen_name: '<自分のTwitterアカウント名(@マークから始まる方の名前を@マーク抜きで入れる)>',
  count: 200
};

// タイムスタンプ取得(ファイル名用)
function getTimestamp(){
  const nowDateObj = new Date();
  return nowDateObj.getTime();
}

client.get('favorites/list', params, (err, tweets, res) => {
  // エラー時の処理
  if(err){
    console.error(err);
    return;
  }
  // 動画URLを抽出する
  for(const content of tweets){
    const extended_media_url = [];
    if(content.extended_entities){
      if(content.extended_entities.media.length){
        for(const media of content.extended_entities.media){
          if(media.video_info){
            for(const video of media.video_info.variants){
              extended_media_url.push(video.url);
            }
          }
        }
      }
    }

    // ツイート情報をまとめる
    content_arr.push({
      id: content.id,
      text: content.text,
      created_at: content.created_at,
      extended_media: extended_media_url
    });
  }

  // 動画ファイルを保存
  (async () => {
    for(const content of content_arr){
      if(content.extended_media.length){
        for(const media_url of content.extended_media){
          if(media_url.match(/\/vid\//g) && media_url.match(/\.mp4/g)){
            const media_filename = getTimestamp() + '.mp4';
            const options = {
              url: media_url,
              encoding: null,
            }
            await rp(options)
              .then(body => {
                const data = new Buffer(body, 'base64');
                fs.writeFileSync('./' + media_filename, data);
                console.log(media_filename + ' を保存しました');
              })
              .catch(err => {
                console.error(err);
              });
          }
        }
      }
    }
  })();
});
```

### ④必要なモジュールをインストール

* ソースファイルが置かれているディレクトリでコマンドプロンプトを開き、下記コマンドでモジュールをインストールしました。
```
$ npm install --save request
$ npm install --save request-promise
$ npm install --save twitter
```
* ディレクトリ内に「package.json」「package-lock.json」ファイルと「node_modules」フォルダができます。

### ⑤実行
* ソースファイルが置かれているディレクトリで下記コマンドを実行しました。
```
$ node app.js
```
※ファイル名が「app.js」の場合  

実行すると、ソースファイルが置かれているディレクトリ内に動画が保存されます。  
サイズ違いで1つの動画につき3種類くらい保存されるみたいですね。  
![]({{site.baseurl}}/assets/img/posts_image/2021-12-10-001/2021-12-10-003.jpg)

## おわりに
デビルスティックが当たっても壊れなさそうなパソコンで動かしてみました。   

これで練習中も安心　やったね！   
![]({{site.baseurl}}/assets/img/posts_image/2021-12-10-001/2021-12-10-004.jpg)