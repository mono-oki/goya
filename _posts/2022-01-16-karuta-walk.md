---
title         : "郷土かるたのオープンデータで散歩コースを作ろう"
---

# はじめに

官公庁や自治体では、交通や観光に関する情報をオープンデータとしてインターネット上で公開しています。

<iframe width="560" height="315" src="https://www.youtube.com/embed/5tVVYrcaT24" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  
  
  
せっかくなので、オープンデータと地図データを組み合わせて簡単な散歩コースを作るWebアプリケーションを作ってみました。

# 準備

## 開発環境

### Node.js

公式ページからインストーラをダウンロードできます。  
[Node.js](https://nodejs.org/ja/){:target="_blank"}

コマンドプロンプト（Macの場合はターミナル）で下記コマンドを打ち、バージョンが表示されてエラーが出なければ大丈夫です。
```
$ node -v
v12.22.3
$ npm -v
8.1.4
```
※↑は筆者の環境です。

### Express

WebアプリケーションのフレームワークとしてExpressを使います。express-generatorを使うと簡単にExpressのアプリケーションひな型を作成できます。  
※GitHubに公開されたソースコードをcloneして動かす場合は、すでにExpressのプロジェクトとして作成されているのでexpress-generatorは不要です。
```
$ npm install -g express-generator
```

下記コマンドでプロジェクトが作成されます。この時点では必要なモジュールがインストールされていないので、インストールするためのコマンドも実行しましょう。
```
$ express --view=ejs <プロジェクト名>
$ npm install
```

`npm start`を実行すると、ローカルの3000番ポートでアプリケーションが実行されます。  
（ブラウザでhttp://localhost:3000/にアクセス）  

## データについて

府中市の公式Webサイトから武蔵府中郷土かるたのオープンデータをCSV形式でダウンロードできます。    
[武蔵府中郷土かるた　東京都府中市ホームページ](https://www.city.fuchu.tokyo.jp/gyosei/fuchusinogaiyo/enkaku/kyodokaruta.html){:target="_blank"}  

CSVデータには、かるたの読み札情報と市内に設置されているかるた標識の位置情報が含まれています。  
今回は下記カラムのデータを使います。  

* 字
* 標識(読み札)
* 設置場所説明
* 住所

ダウンロードしたデータには標識の緯度経度情報は含まれていなかったため、府中市公式Webサイトで公開されている標識マップをもとに自前で用意した緯度経度情報を使いました。  
[https://fugis.city.fuchu.tokyo.jp/FuchuInternet/portal/karuta.html](https://fugis.city.fuchu.tokyo.jp/FuchuInternet/portal/karuta.html){:target="_blank"}  

## 実装

### ソースコード

GitHubに公開しました。  
[https://github.com/Imagawayaki/karutaWalk](https://github.com/Imagawayaki/karutaWalk){:target="_blank"}

ビルド手順やアプリケーションの実行方法をREADMEに記載しました。  
動けば良かろうの精神で作ったので、出発／到着駅・かるたの情報はハードコーディングしています。。  

### 使ったもの

アプリケーションのバックエンド側はNode.jsとExpressで開発し、フロント側はBootstrapとOnsenUIで見た目を整えています。  
[Bootstrap](https://getbootstrap.jp/){:target="_blank"}  
[OnsenUI](https://ja.onsen.io/){:target="_blank"}  

マップの表示はLeafletとOpenStreetMap、ルート検索はLeaflet Routing Machineを使用しています。  
[OpenStreetMap](https://www.openstreetmap.org/){:target="_blank"}  
[Leaflet](https://leafletjs.com/){:target="_blank"}  
[Leaflet Routing Machine](https://www.liedman.net/leaflet-routing-machine/){:target="_blank"}  

## 使ってみる

①`npm start`コマンドをcloneしたリポジトリのルート上で実行し、アプリケーションのTOPページ（ローカルで動かしている場合は http://localhost:3000 ）にアクセスします。  

②「出発駅」「到着駅」「かるた」をプルダウンメニューから選択して「表示」ボタンをクリックします。  

![]({{site.baseurl}}/assets/img/posts_image/2022-01-16-001/2022-01-16-001.png)

③出発駅から到着駅までのルートが表示されます。ルートは選択したかるたの標識を経由しています。  

![]({{site.baseurl}}/assets/img/posts_image/2022-01-16-001/2022-01-16-002.png)

## おわりに
 
インターネット上に様々なオープンデータが公開されているので、いろいろなアイデアに活用できそうで楽しそうですね。  
これからもドンドン触ってみたいと思います。  

終り  
