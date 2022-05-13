---
title: "（今更だけど）cookieのdomain属性について少し理解を深めた"
emoji: "🍪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['JavaScript', 'cookie','web','http']
published: true
---

# はじめに
最近cookieについて理解が薄いなと感じる機会があり、「webエンジニアとして致命的では？」と思ったので調べました。今更。  
domain属性の理解に時間がかかったので、今回はdomain属性について記載していきます。  

# 目次
* domain属性って何
* domain属性の一致判定
* コンソールで試してみる

## domain属性って何
domain属性ではcookieを送信する対象のURLを定義できます。  
domain属性に設定したdomainと送信先のdomainが一致していれば送信できます。

例を挙げると
cookieをセットしたdomainが
```
example.com
```
送信先のAPIのエンドポイントが
```
example.com
```
であればリクエストヘッダーにcookieをセット可能です。
```
example.com
```
送信先のAPIのエンドポイントが
```
hoge.example.com
```
であればリクエストヘッダーにcookieをセットすることはできません。

## domain属性の一致判定
domain属性の一致判定には「完全一致」と「後方一致」の2種類あります。  
domain属性未指定の際は「完全一致」、domain属性を定義すれば「後方一致」となります。  

つまり安全性の観点ではdomain属性を指定しない方が、完全一致で判定するため安全性は高いです。  

### domain未指定の場合
* cookieのdomainにはcookieにセットした際のdomainが設定される
* また一致判定は「完全一致」
* 完全一致なので安全性は高い

| cookieのdomain | 送信先のdomain | cookieセットの可否 |
| ---- | ---- | :---: |
| hoge.example.com | hoge.example.com | ◯ |
| hoge.example.com | example.com | × |
| example.com | hoge.example.com | × |

### domain指定の場合
* cookieのdomainには指定したdomainが設定される
* また一致判定は「後方一致」

| cookieのdomain | 送信先のdomain | cookieセットの可否 |
| ---- | ---- | :---: |
| hoge.example.com | hoge.example.com | ◯ |
| hoge.example.com | example.com | × |
| example.com | hoge.example.com | ◯ |


## コンソールで試してみる
実際に試してみましょう。  
chromeのdevToolを用いれば簡単に確認できます。  

ドメインがわかりやすいように
`F12` でdevToolsを開き、コンソール(画像右下赤枠)とアプリケーション(画像右上赤枠)を開きます。

![](https://storage.googleapis.com/zenn-user-upload/b16945fdf547-20220513.png)　

cookieをセットしてみます。　　
コンソール画面で以下のscriptを実行します。
```js
document.cookie = "test_key=test_value;";
```
するとアプリケーション画面でcookieがセットできていることが確認できます。　　
cookieのdomainは `www.google.com` となっており現在のホストのdomainがセットされています。  
![](https://storage.googleapis.com/zenn-user-upload/0d00372a14c7-20220513.png)

cookieのバリューを変更します。  
コンソール画面で以下のscriptを実行します。
```js
document.cookie = "test_key=test_value_2;";
```
するとcookieのvalueが更新されました。  
![](https://storage.googleapis.com/zenn-user-upload/3ae2148be2b2-20220513.png)

cookieのドメインにexampleを指定してvalueの更新を試みます。  
```js
document.cookie = "test_key=test_value_3; domain=examlpe";
```
今回はvalueが更新されませんでした。
これは指定した `example` と `google.com` が異なっているためです。  
![](https://storage.googleapis.com/zenn-user-upload/2e543e656a65-20220513.png)

このようにdomain指定したcookieや未指定のcookieのセットを実行して簡単な挙動を試せます。

# おわりに
cookieってよく使う割にライブラリに頼りがちです。  
自分みたいに何も理解できていないと「なぜかcookieがdev環境でだけRequestHeaderに付与されない」「なぜかcookieか削除できない」ってことになります。  
理解しないまま実装した最悪の場合、[cookieを使用した攻撃](https://securitynews.so-net.ne.jp/topics/sec_20172.html)の標的になり損失につながりかねませんね。こわ。  
ライブラリやフレームワークより、こういう基礎の学習って大事だなと思いました。  

# 参考記事
https://qiita.com/HAYASHI-Masayuki/items/209039717c15834603d8
https://kostum.hatenablog.jp/entry/2022/05/04/000000
https://triple-underscore.github.io/http-cookie-ja.html#sane-domain
