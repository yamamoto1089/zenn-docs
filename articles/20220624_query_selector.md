---
title: "idやclass以外の属性から要素を取得したい時"
emoji: "♲"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['html','javascript']
published: false
---

# はじめに

```html
<div id="id_name" class="class_name" hoge="fuga">
```
例えばこのような要素がある場合、

```js
document.getElementById('id_name');
```
とか
```js
document.getElementsByClassName('class_name');
```
で要素を取得することは可能です。  

hogeの属性で要素を取得する方法について、知らなかったので備忘録として残すことにしました。  

# 結論
```html
<div id="id_name" class="class_name" hoge="fuga">
```
idやclass属性以外から取得する場合は`querySelector`取得できます。

```js
document.querySelector('[hoge="fuga"]');
```

## querySelectorとは

https://developer.mozilla.org/ja/docs/Web/API/Document/querySelector
> 指定されたセレクターまたはセレクター群に一致する、文書内の最初の Element を返します。一致するものが見つからない場合は null を返します。

querySelectorで指定した文字列に一致する要素を返却してくれる様です。  
文字列に一致する全ての要素を取得したい場合は[querySelectorAll](https://developer.mozilla.org/ja/docs/Web/API/Document/querySelectorAll)を使います。

セレクターの指定方法次第ではかなり詳細に要素を絞り込んで取得できそうです。

# 余談
本題から逸れますが、なぜidやclass以外の属性から要素を取得する必要があったかについて覚えてる範囲で書いていきます。  

稀にサイト上にスニペットタグを埋め込む必要があります。  
↓GTMのスニペットタグ（サンプル）
```html
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'//www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXX');</script>
```
スニペットタグのscriptが走れば内部に書かれているjsが実行されます。
上記のGTMの例では、「Googleタグマネージャを呼び出すJavaScript」が実行されてGTMが利用可能になります。  

今回自分が埋め込んだスニペットのscriptには、「外部jsファイルを読み込むscriptタグをhead要素に追加する」処理がありました。  
その画面でしか必要ないscriptタグですが、外部ファイル読み込み後もscriptタグが残り続けるのは少し嫌です。  
そこで外部ファイル読み込み後にheadに追加されたscriptタグを削除しようと思いましたが、埋め込まれたscriptタグにはidやclass属性がなく、srcしか記載されていませんでした。。。
```html
<script async src="https://xxx.js"></script>
```

ここで対応策は2つあり、「スニペットのscriptにid属性を追加してgetElementByIdで要素を取得して削除」か「idやclass以外の属性から要素を取得できる方法を探す」です。

* スニペットのscriptにid属性を追加してgetElementByIdで要素を取得して削除  
  先程のGTMのサンプルを例にとると`j.id='hoge';`を追加してid属性を付与すれば可能です。
```html
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.id='hoge';j.src=
'//www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXX');</script>
```
しかし、id属性を追加するだけとはいえ、提供されているスニペット内部を変更することに抵抗があります。可能であればスニペット内に変更を加えずに要素を取得したい。。。

* idやclass以外の属性から要素を取得できる方法を探す  
  ここで記事の本題に戻ってきます。

# おわりに
querySelectorを使うという初歩的なことで解決できました。  
こういう簡単にできそうなのに解決方法が出てこないって時がたまにあるんですが、ほとんど基本的な知識で解決できることが多いです。  
もっと基本的なところ勉強しないとなと。。。
