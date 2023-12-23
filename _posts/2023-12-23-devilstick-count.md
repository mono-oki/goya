---
title         : "WT901BLECLを使ってデビルスティックのアイドリング回数カウントシステムを試作してみました"
---

## はじめに
こんにちは。Imagawayakiと申します。  
この記事は、[ジャグリング Advent Calendar 2023](https://adventar.org/calendars/8710){:target="_blank"} の記事として投稿しました。  

## やったこと
Amazonで面白そうなデバイスを見つけたので、デビルスティックと組み合わせてアイドリングの回数をカウントできるか試してみました。  
[[5.0加速度計+傾斜計] WT901BLECL MPU9250高精度9軸ジャイロスコープ+角度（XY0.05°精度）+カルマンフィルター付き磁力計、の低電力3軸AHRS IMUセンサー](https://www.amazon.co.jp/Bluetooth5-0%E5%8A%A0%E9%80%9F%E5%BA%A6%E8%A8%88-WT901BLECL-MPU9250%E9%AB%98%E7%B2%BE%E5%BA%A69%E8%BB%B8%E3%82%B8%E3%83%A3%E3%82%A4%E3%83%AD%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%97-%E8%A7%92%E5%BA%A6%EF%BC%88XY0-05%C2%B0%E7%B2%BE%E5%BA%A6%EF%BC%89-%E3%82%AB%E3%83%AB%E3%83%9E%E3%83%B3%E3%83%95%E3%82%A3%E3%83%AB%E3%82%BF%E3%83%BC%E4%BB%98%E3%81%8D%E7%A3%81%E5%8A%9B%E8%A8%88%E3%80%81%E3%81%AE%E4%BD%8E%E9%9B%BB%E5%8A%9B3%E8%BB%B8AHRS/dp/B07SYYRZQ2){:target="_blank"}  

※(念のため)初めてデビルスティックを見る方向けコメント  

下記の動画で紹介されているジャグリング道具が「デビルスティック」です。  
<iframe width="560" height="315" src="https://www.youtube.com/embed/HhfULKjZ2kE?si=KRinEs8dwWkupvYI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

「アイドリング」はデビルスティックの基本技になります。  
今回は片方に傾いた回数を1回としてアイドリングの回数をカウントしていきます。  

※デビルスティックの紹介は下記動画から引用しました。  
[【デビルスティック講座 初級編】第2回 アイドリング](https://youtu.be/HhfULKjZ2kE?si=fi_kdmM43fe2vJdo){:target="_blank"}

## 今回使ったセンサー
WT901BLECLは加速度センサーとジャイロセンサーで傾斜を測ることができ、計測データをBLE(Blutooth Low Energy)でPCやスマートフォンに送信できます。  
デビルスティックに限らずジャグリング道具は空中を浮遊するため、何かしらの通信をする場合は無線だと便利です。  
BLEを使うと低い消費電力で無線通信できるため、今回のケースにはもってこいですね。  

[センサーメーカーの公式サイト](https://www.wit-motion.com/9-axis/wt901blecl-mpu9250-high-precision.html){:target="_blank"}にはAndroidやLinuxのサンプルプログラムが公開されているのですが、今回はWindowsで動かしたいため下記サイトで紹介されているBleakというPythonのライブラリを使う方法を参考にしてみました。   

※参考サイト: [WT901BLECLにBLE接続してデータを読み取る](https://zenn.dev/fastriver/articles/wt901blecl_read_data){:target="_blank"}  

センサーは様々な値を取得してくれるのですが、スティックの傾きが分かればアイドリングのカウントができそうだったので、今回はazの値(z軸の傾き) だけを使い、傾きが一定の角度より水平に近くなったらカウントを1増やすようにしました。  

## 実際に動かしてみた
まずは手でスティックを傾けてカウントが増えるか試してみました。([動画リンク](https://youtu.be/2UYgqZz3BmA?si=gP3F90RjWrMA5BOv){:target="_blank"})  
<iframe width="560" height="315" src="https://www.youtube.com/embed/2UYgqZz3BmA?si=gP3F90RjWrMA5BOv" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

傾きから回数は取れていそうですね。  

---

今度は実際にアイドリングしてみます。([動画リンク](https://youtu.be/dO3qWxZ_wnA?si=7vDpcehry4bsZviZ){:target="_blank"})  
<iframe width="560" height="315" src="https://www.youtube.com/embed/dO3qWxZ_wnA?si=7vDpcehry4bsZviZ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

何となくアイドリングの回数が取れているように見えていますね。  

アイドリングによる振動でセンサーの値がぶれるため、丁寧にアイドリングしてあげないとカウントが増えていかないことがあります。  
多少強くスティッキングしてもカウントが増えるようにすればアイドリング以外の技も回数カウントできそうですが、今後の課題ですね。   

## おわりに
今回使ったセンサーは他にもx軸・y軸の傾きなど色々なデータが取れるので、より精度良くアイドリング回数をカウントできないか試行錯誤してみます。  

良さそうな方法があれば是非教えてください！  

終り  
