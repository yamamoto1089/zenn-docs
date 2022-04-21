---
title: "簡易的なブックマークレットを作ってみた"
emoji: "📖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["javascript","bookmarklet","Chrome"]
published: false
---

# はじめに

ブックマークレットを作成することで、ブラウザのブックマークから簡単なJavaScriptのコードを実行できます。  

今回は以下のようなZennの記事を検索できるブックマークレットを作成してみます。
![](https://storage.googleapis.com/zenn-user-upload/3f5f3bfbeb35-20220422.gif)

# そもそもブックマークレットとは

ブラウザのアドレスバーは一般的にURLが表示されています。  
![](https://storage.googleapis.com/zenn-user-upload/5b5da03dbef7-20220420.png)

このアドレスバーに `javascript:` と入力することでJavaScriptのコードが実行できます。    
ブックマークレットとは `javascript:` からはじまるコード自体をブックマークとして登録し、簡易的にコードを実行できるようにしたものです。

簡単な例として以下をブラウザのアドレスバーに入力してみます。  
※アドレスバーにコピペする場合、`javascript:` の部分がペーストされないので注意です。  

```js
javascript: alert('hello world');
```
![](https://storage.googleapis.com/zenn-user-upload/70f997e95e67-20220420.png)

入力後にエンターを押すとコードが実行され、アラートで `hello world` と表示されたはずです。  

![](https://storage.googleapis.com/zenn-user-upload/0677edc1e94a-20220420.png)

最後に入力した `javascript: alert('hello world');` をブックマークに登録すれば、ブックマークレットの完成です。  

ここからはいくつかのブックマークレットを作成していきます。

# 手順

* 検索画面のURLを見てみる
* 任意のワードを入力してみる
  * window.open
  * window.prompt
  * getSelection
* ブックマークへ追加
* 完成物一覧
  * 任意の検索ワードを入力しZennで検索する
  * 選択中の文字列をZennで検索する
  * 選択中の文字列をGoogle翻訳で開く（日本語->英語）
  * 選択中の文字列をGoogle翻訳で開く（英語->日本語）

# 検索画面のURLを見てみる

はじめに例に挙げた「Zennの記事を検索できるブックマークレット」を作成した手順について記述していきます。  

まずは[Zennの検索ページ](https://zenn.dev/search)に遷移します。
```shell:URL
https://zenn.dev/search
```

ここで「frontend」という単語で検索をかけてみると、アドレスバーには次のように表示されました。
```shell:URL
https://zenn.dev/search?q=frontend
```

ここでわかることは、入力された**検索ワード**は `q={検索ワード}` というクエリでURLに渡されるということです。  
つまり `https://zenn.dev/search?q={検索ワード}` の `検索ワード` に任意のワードを渡せばZennの記事を検索できそうです。

# 任意のワードを入力して別タブで開く

## window.open

まずは指定したURLをJavaScriptで開けるようにします。
[window.open](https://developer.mozilla.org/ja/docs/Web/API/Window/open)を使用することでURLを別タブで開けます。

```js
javascript: window.open('https://zenn.dev/search?q={検索ワード}');
```

## window.prompt

任意の検索ワードをURLの `{検索ワード}` 部分に渡します。　　
方法はいくつかありそうですが、今回は[window.prompt](https://developer.mozilla.org/ja/docs/Web/API/Window/prompt)を使用します。  
`window.prompt();` を実行すると、テキスト入力フィールドを持ったダイアログが表示されます。  
テキスト入力フィールドに入力した値がそのまま返却されるので、`window.prompt()` を `{検索ワード}` 部分に埋め込みます。

```js
javascript: window.open('https://zenn.dev/search?q='+window.prompt());
```

ダイアログが少し味気ないので、表示テキストを追加してみます。  

```js
javascript: window.open('https://zenn.dev/search?q='+window.prompt('zennで検索します','検索ワード'));
```

これでコード自体は完成です。

## getSelection

余談ですが、文字列を取得する方法として[getSelection](https://developer.mozilla.org/ja/docs/Web/API/Window/getSelection)があります。　　
選択中の文字列を取得できるので、「選択中の文字列をZennで検索する」ことができます。

```js
javascript: window.open('https://zenn.dev/search?q='+getSelection().toString());
```

# ブックマークへ追加

作成したコードがアドレスバー上で実行できたらブックマークへ登録します。

右上の3点リーダーから `ブックマーク->ブックマーク マネージャ` を選択します。
![](https://storage.googleapis.com/zenn-user-upload/ebb0d8258ede-20220422.png)

ブックマークマネージャ上の右上3点リーダから `新しいブックマークを追加` を選択します。
![](https://storage.googleapis.com/zenn-user-upload/857f671ead2d-20220422.png)

名前に任意の名前、URLに `javascript:` から始まるコードを貼り付ければ完成です。
![](https://storage.googleapis.com/zenn-user-upload/3fb03ec9e077-20220422.png)

これでブックマークから実行できるようになりました。

# 完成物一覧

## 任意の検索ワードを入力しZennで検索する
```js
javascript: window.open('https://zenn.dev/search?q='+window.prompt('zennで検索します','検索ワード'));
```
![](https://storage.googleapis.com/zenn-user-upload/3f5f3bfbeb35-20220422.gif)

## 選択中の文字列をZennで検索する
```js
javascript: window.open('https://zenn.dev/search?q='+getSelection().toString());
```
![](https://storage.googleapis.com/zenn-user-upload/a141decfbcc8-20220422.gif)

## 選択中の文字列をGoogle翻訳で開く（日本語->英語）
```js
javascript: window.open('https://translate.google.co.jp/?hl=ja&sl=ja&tl=en&text=' getSelection().toString() '&op=translate');
```
![](https://storage.googleapis.com/zenn-user-upload/fa77dd7bc585-20220422.gif)


## 選択中の文字列をGoogle翻訳で開く（英語->日本語）
```js
javascript: window.open('https://translate.google.co.jp/?hl=ja&sl=en&tl=ja&text=' getSelection().toString() '&op=translate');
```
![](https://storage.googleapis.com/zenn-user-upload/a1e1836d3ffb-20220422.gif)

# おわりに
ブックマークレットではJavaScriptのコードを簡単に実行できました。  

ブックマークレットを共有するだけで他人でも同じコードを実行できることもブックマークレットの利点です。例えば、ログイン処理を行うブックマークレットを作成すれば、ワンクリックでログインができ、案件内でそのログイン処理を共有することもできそうです。

windowインターフェイスの組み合わせやコードの次第で、より便利なブックマークレットを作成できそうです。  

# 参考記事
https://kotamat.com/post/bookmarklet-tips/
