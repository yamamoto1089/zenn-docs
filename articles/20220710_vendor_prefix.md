---
title: "ベンダープレフィックスについて調べてみた"
emoji: "🖥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['css','html','javascript','ブラウザ']
published: false
---

# はじめに

普段cssをそれほど触らないこともあり、cssのベンダープレフィックス（`-webkit-` とか `-moz-`)についてよくわかっていなかったので調べてみました。  

# ベンダープレフィックスとは

> ブラウザーベンダー (提供元) は、時に試験的または非標準な CSS プロパティおよび JavaScript API に接頭辞を追加することがあります。これにより、開発者は標準化プロセスの中で、理論上は、ウェブ開発者のコードを壊すことなく新しいアイデアを試すことができます。

引用:https://developer.mozilla.org/ja/docs/Glossary/Vendor_Prefix

要するにブラウザで標準化されていない非標準のCSSプロパティを使用する場合に、プレフィックスを追加することで利用できるようになるということです。  
ベンダープレフィックスはブラウザによって形式が異なっており、主なプレフィックスと対応ブラウザは以下になります。  
※詳しくは後述。

| ベンダープレフィックス | ブラウザ | 備考 |
| ---- | ---- | ---- |
| -webkit- | Chrome<br>Safari<br>Opera(バージョン15以降)<br>MicrosoftEdge(Chromium採用以降)<br>ほぼすべての iOS ブラウザー (Firefox for iOS を含む) |  |
| -moz- | Firefox |  |
| -o- | Opera(バージョン15以前) | Operaはバージョン15(2013年2月13日リリース)からChromiumベースに作り直し、レンダリングエンジンもwebkitに変更。よって現在はあまり使われない。 |
| -ms- | InternetExplorer <br>MicrosoftEdge(Chromium採用前) | InternetExplorerは2022年6月15日にサポート終了。<br>MicrosoftEdgeは2020年1月15日にChromiumを採用。よって現在はあまり使われない。  |

ベンダープレフィックスの使用例
```css
-webkit-transition: all 4s ease;
-moz-transition: all 4s ease;
-ms-transition: all 4s ease;
-o-transition: all 4s ease;
transition: all 4s ease; 
```

# ベンダープレフィックスとレンダリングエンジンの関係
ベンダープレフィックスがブラウザごとに違うのはレンダリングエンジンと関係があります。  
レンダリングの仕組みについては[こちら](https://zenn.dev/oreo2990/articles/280d39a45c203e)の記事が参考になりました。  

## 各ブラウザとレンダリングエンジンの歴史
以前はそれぞれのブラウザで異なるレンダリングエンジンを採用していた様です。  
主要ブラウザがそれぞれ別のレンダリングエンジンを採用していたがために、ベンダープレフィックスも多くありました。

### 以前のレンダリングエンジンとブラウザ
| ブラウザ | レンダリングエンジン | ベンダープレフィックス |
| ---- | ---- | ---- |
| Chrome | Webkit | -webkit- |
| Safari | Webkit | -webkit- |
| Opera | Presto | -o- |
| Firefox | Gecko | -moz- |
| InternetExplorer | Trident | -ms- |
| MicrosoftEdge | EdgeHTML | -ms- |

### Chromiumの導入
しかし[Chromium](https://ja.wikipedia.org/wiki/Chromium)を採用するブラウザが増えたことで、徐々にレンダリングエンジンも統一されていきました。  
ChromiumはChromeのブラウザを作成するベースにもなっているため、レンダリングエンジンにはWebkitが使用されていました。  
まずは2013年2月13日にOperaがChromiumベースに乗り換え、2020年1月15日にMicrosoftEdgeがChromium版をリリースしました。  
これにより主要ブラウザで使用されるレンダリングエンジンからPrestoとEdgeHTMLが消えることになりました。2022年6月15日にはInternetExplorerのサポートが終了しました。  
実質的にベンダープレフィックスから `-o-`,`-ms-` が使用されることはほぼなくなります。  

後にChromiumはレンダリングエンジンをWebkitからBlinkに乗り換えます。  
BlinkはWebkitからフォークされたレンダリングエンジンなのでベンダープレフィックスは `-webkit-` が使用できます。  

この様な背景から、現在の主要なブラウザとレンダリングエンジンは以下の様になっています。  

### **現在のレンダリングエンジンとブラウザ**
| ブラウザ | レンダリングエンジン | ベンダープレフィックス |
| ---- | ---- | ---- |
| Chrome | Blink | -webkit- |
| Safari | Webkit | -webkit- |
| Opera | Blink | -webkit- |
| Firefox | Gecko | -moz- |
| InternetExplorer | Trident | -ms- |
| MicrosoftEdge | Blink | -webkit- |

これからの開発においては `-webkit-` と `-moz-` のみ意識することになりそうです。

# ベンダープレフィックスの必要性の調査
ベンダープレフィックスをつける前に、`-webkit-` と `-moz-` が必要なプロパティかどうかを調査する必要があります。  

## MDNで確認
基本的にはMDNでプロパティを調べると、ブラウザの互換性の欄に「どのブラウザでサポートされているか」や「どのベンダープレフィックスが必要か」まで記載されています。  
![](https://storage.googleapis.com/zenn-user-upload/ff3be11871c1-20220710.png)


## Can I Use... で確認
他にも[Can I Use ...](https://caniuse.com/)を利用するのも便利です。(会社の人に教えてもらいました)  
確認したいプロパティ名を入力するだけて各ブラウザのサポート状況やベンダープレフィックスが必要かを表示してくれます。  
バージョンごとにまとめられた表があるので、こちらの方がUI的に確認しやすいです。  
![](https://storage.googleapis.com/zenn-user-upload/b98e487ca6bf-20220710.png)

# おわりに  
ベンダープレフィックスについて調べることでブラウザの歴史やレンダリングエンジン採用の歴史について少し理解が深まった気がします。  
一方でそもそもベンダープレフィックスが必要な状況に違和感も感じました。  

> ブラウザーベンダーは、実験的な機能にベンダー接頭辞をつけることをやめるようになってきています。ウェブ開発者は、実験的な機能であるにもかかわらず、実運用のウェブサイトで使用し続けてきました。これはブラウザーベンダーが互換性を維持して、新しい機能を導入することを困難にしてしまいました。これはシェアの小さなブラウザーにとっても有害で、有名なウェブサイトを読み込むために他のブラウザーの接頭辞を追加せざるを得ない結果になりました。

引用:https://developer.mozilla.org/ja/docs/Glossary/Vendor_Prefix

公式でも言っているようにベンダープレフィックスを付ける必要があること自体に問題がある気がします。  
実装やPRレビューの際に調査に時間を使いたくないですし。。。  
InternetExplorerもお亡くなりになりましたし、そのうちベンダープレフィックスを考える必要のない、優しい世界になってほしいです。  

以前読んだ[2000年代のHTMLコーディングの記事](https://ics.media/entry/17960/)を読めばワガママも言ってられないとも思いますが。。。


# 参考
https://qiita.com/umashiba/items/8cb47825624c5cb043d6
https://zenn.dev/azunasu/articles/581153b9ec1f92
https://developer.mozilla.org/ja/docs/Glossary/Vendor_Prefix
http://www.htmq.com/csskihon/603.shtml
https://ja.wikipedia.org/wiki/HTML%E3%83%AC%E3%83%B3%E3%83%80%E3%83%AA%E3%83%B3%E3%82%B0%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%B3
https://zenn.dev/oreo2990/articles/280d39a45c203e
