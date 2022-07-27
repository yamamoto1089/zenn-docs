---
title: "safariのレスポンシブ・デザイン・モードを活用する"
emoji: "🧭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['safari','devtools']
published: false
---

# はじめに

フロントの開発をしていると、iOSのsafariの表示が確認したい時があります。  
普段はiOS端末を用意して表示確認してたんですが、簡単な表示であればmacのsafariから挙動を確認できるみたいです。
(普段chromeしか使ってないから知らなかった。。。)

# 設定

## safariのツールバーに開発メニューを表示させる

ツールバーから、`safari`→`開発環境...`→`詳細` を選択し `メニューバーに"開発"メニューを表示` にチェックを入れる
![](https://storage.googleapis.com/zenn-user-upload/ed25a3d8ff73-20220727.png)

## レスポンシブ・デザイン・モードの起動

ツールバーから開発を選択し、`レスポンシブ・デザイン・モードにする` を選択
![](https://storage.googleapis.com/zenn-user-upload/e2a3ea9c01a9-20220727.png)

# レスポンシブ・デザイン・モードでできること

## 端末のシミュレート

`iPhone SE` や `iPhone 8` だけでなく `iPad`,`iPad Pro` のインチ違いも選択できます。


端末のアイコンをもう一度クリックすることで横画面表示も確認できます。

## ブラウザのシミュレート
右上の選択肢を開くとSafariの表示だけではなく、MicrosoftEdge,Google Chrome, Firefoxが選択できます。  
![](https://storage.googleapis.com/zenn-user-upload/b44bd7e47664-20220727.png)

しかもChromeとFirefoxに関してはmacOSとWindowsまで選択できます。  

Windows
![](https://storage.googleapis.com/zenn-user-upload/0cb9af3b30b4-20220727.png)

macOS
![](https://storage.googleapis.com/zenn-user-upload/7a5088066fdb-20220727.png)

## iOSバージョンのシミュレート

同じく右上の選択肢からiOSのバージョンも指定できます。  
![](https://storage.googleapis.com/zenn-user-upload/512dcb61c728-20220727.png)

## Retinaディスプレイのシミュレート

上部真ん中の選択肢でRetinaディスプレイのシミュレートができます。  
![](https://storage.googleapis.com/zenn-user-upload/4fb651e9e882-20220728.png)

Retinaディスプレイについては[こちら](https://www.koushinclub.com/blog/2038.html)の記事が参考になりましたが、1ドットあたりのピクセル数をシミュレートできるようです。  

1x
![](https://storage.googleapis.com/zenn-user-upload/668a6515c13a-20220728.png)

3x
![](https://storage.googleapis.com/zenn-user-upload/668a6515c13a-20220728.png)

# おわりに

今回のもそうだけど、ブラウザの開発ツール知らなすぎて時間無駄にしてる気がする。。。
