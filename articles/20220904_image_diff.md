---
title: "画像の差分検出でimageMagickを使ってみた"
emoji: "🫥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['HTML','design','imageMagick']
published: true
---
# はじめに

web制作ではデザインに合わせてマークアップを行うため、最終的にブラウザに表示される画面がデザイン通りになっているか確認する必要があります。  
また、既存サイトのデザイン修正を行う場合でもデザインのどこが変更されたのかを確認する必要があります。  

このようにデザインを比較する場合、目視で変更点を確認するのは辛いので[imageMagick](https://imagemagick.org/index.php)を試してみます。

# imageMagickインストール

インストール方法は[公式サイト](https://imagemagick.org/script/download.php)にいくつか記載されていますが、Homebrewでインストールします。

```
brew install imagemagick
```

一応インストールされたことを確認。

```
convert -version
```

# compositeで画像の差分を試す

composieで画像の比較ができます。  

```
composite -compose difference {比較画像A} {比較画像B} {出力結果}
```

## 差分がない場合

比較箇所が同じ色であるほど黒色に近づくように出色されるため、差分がない場合は出力される画像が真っ黒になります。  
試しに同じ画像同士を比較させます。  

```
composite -compose difference imageA.jpeg imageA.jpeg diff.jpg
```

比較画像  imageA.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/97f28a111eef-20220904.jpeg)


出力結果  diff.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/754fe320c728-20220904.jpg)


## 差分がある場合

比較箇所で色に差分がある箇所は黒色以外で出力されます。  

比較画像  imageA.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/97f28a111eef-20220904.jpeg)

比較画像  imageB.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/1b5d45f63022-20220904.jpeg)

出力結果  diff.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/1e63fd3ba3f7-20220904.jpg)

# 実際のサイトで試してみる

実際にサイトの画像で試してみます。  
サンプル画像にZennのプロフィール画面を使用します。  

※Webページ全体をキャプチャするためにChromeの拡張機能の[GoWebPage](https://chrome.google.com/webstore/detail/gofullpage-full-page-scre/fdpohaocaechififmbbbbbknoalclacl?hl=ja)を使用します   


## マージンなどは変わらず文字や画像のみ変更がある場合

文字や絵文字の一部を変更して比較してみます。  

```
composite -compose difference Zenn_A.jpeg Zenn_B.jpeg diff.jpg
```

比較画像  Zenn_A.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/4fd49ba95d37-20220904.png)

比較画像  Zenn_B.jpeg  絵文字部分や文字を少しだけ変更したもの  
![](https://storage.googleapis.com/zenn-user-upload/87272eb12e08-20220904.png)

出力結果  diff.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/c4ada0a13de5-20220904.jpg)

文字や絵文字の変更部分のみが出力されるので変更箇所が一目でわかります。  

## マージンに変更がある場合

文字などはそのままでページ上部の要素のマージンのみを変更してみます。  

```
composite -compose difference Zenn_A.jpeg Zenn_C.jpeg diff.jpg
```

比較画像  Zenn_A.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/4fd49ba95d37-20220904.png)

比較画像  Zenn_C.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/a1c26edb9a3b-20220904.png)

出力結果  diff.jpeg  
![](https://storage.googleapis.com/zenn-user-upload/610b6273e7ac-20220904.jpg)

マージンなどが変更されている場合は差分箇所が多く出力されてしまい、ずれを検出する方法としては実用的ではないですね。

# おわりに
デザインの比較という目的ではimageMagickが使える機会はそんなに多くなさそう。。。  
そもそも軽微なデザイン修正とか文言修正はコミュニケーションでなんとかなる気がします。  

結局は目視の確認が必要なので画像で比較するとかよりも[visual-inspector](https://chrome.google.com/webstore/detail/visual-inspector/efaejpgmekdkcngpbghnpcmbpbngoclc)みたいなChromeの拡張を利用するのが良い気がしますね。  
と思って調べてたらvisual-inspectorがChromeのポリシー違反で消されてた。。。


# 参考記事
https://tektektech.com/mac-imagemagick-install/
https://blog.mirakui.com/entry/20110326/1301111196
https://ics.media/entry/220331/
https://docodoor.co.jp/staffblog/chrome_extensions/
