---
title: "JavaScriptにおける丸め誤差と対応"
emoji: "📐"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['javascript','big.js','bignumber.js','decimal.js']
published: true
---

# 丸め誤差とは
小数点以下の計算を行った際に発生する誤差のことです。  
Chromeのコンソール画面で確認してみます。  
![](https://storage.googleapis.com/zenn-user-upload/caf816e6f22d-20220610.png)
実行結果として `0.1` になることを期待していましたが、`0.09999999999999998` になってしまいました。
小数点以下の計算自体あまり行うことはないかもしれませんが、外貨計算など金額を扱う際には必要になってきます。  
今回はその原因と丸め誤差に対応するJavaScriptライブラリ(big.js,bignumber.js,decimal.js)の選定方法について記載していきます。  

# なぜ丸め誤差が起きるか

## IEEE754の仕様
浮動小数点数の計算には[IEEE754(浮動小数点数算術標準)](https://ja.wikipedia.org/wiki/IEEE_754)が使われています。  
小数点以下の数字は2進数で表され、浮動小数点の計算も2進数で行われます。  
その際に2進数で表せない数の場合は近似値で表されることになり、誤差が生じます。
この近似値は実際の数値と比べて多少誤差がある上に、近似値同士で計算された場合に誤差が顕著に現れます。  
その結果、さっきの例のように、

```js
0.3-0.2
-> 0.09999999999999998
```
となり、期待値の0.1から `0.00000000000000002` ほど誤差が生まれます。  

IEEE754に準じている以上、誤差が生まれることは正しいです。　　
またIEEE754はさまざまなシステムが採用しているのでJavaScriptに限った話ではないです。  

## JavaScriptのNumber型は小数点何桁までの精度を保証しているか 

ではJavaScriptのNumber型は小数点何桁までの精度を保証しているのでしょうか。  
[MDNのNumberの項目](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Number)の記載を見ると**約17桁**までのようです。 

> Number は小数点以下約 17 桁の精度しか保持できません。演算は丸め誤差の影響を受けます。

実際に試してみると小数点以下19桁で丸め誤差が発生していました。
![](https://storage.googleapis.com/zenn-user-upload/7697acb1621b-20220610.png)
数値が2進数においてどのくらいの近似値で表されるかによって何桁で丸め誤差が発生するかは違ってきます。  
なのでMDNでも**約17桁**という表現なんですかね。

# 丸め誤差の対応

丸め誤差の対応方法としては、

* 小数点を整数に直して計算する
* ライブラリを使用する  

の2つがあると思っています。

## 小数点を整数に直して計算する
そもそも丸め誤差が発生するのは浮動小数点が2進数で正しく表せないためです。なので一度整数に置き換えて計算し、計算結果を再び小数点に戻すというやり方です。  
局所的に丸め誤差を回避したい場合などはこのやり方で良いかもしれませんが、かなり手間な上に整数⇄小数点の変換でミスを起こしそうなのでお勧めはしません。  

## ライブラリを使用する
JavaScriptのライブラリでは計算に特化したライブラリがいくつかあり、これらのライブラリは丸め誤差についても考慮されています。

npm trendsでライブラリを比較してみました。  
**big.js vs bignumber.js vs decimal.js vs mathjs vs numeraljs**  
https://www.npmtrends.com/big.js-vs-bignumber.js-vs-mathjs-vs-numeraljs-vs-decimal.js
比較結果を見る限り `big.js`、`bignumber.js`、`decimal.js` あたりがよく使われているようですね。  

でもそれぞれのライブラリはどのように使い分けを行うのでしょうか？  
試しにbig.jsのREADMEを読んでみると、使い分けに関するヒントが書いてありました。  
> The little sister to bignumber.js and decimal.js. See [here](https://github.com/MikeMcl/big.js/wiki) for some notes on the difference between them.

そもそもこの3つのライブラリの製作者は[MikeMcl](https://github.com/MikeMcl)という同一人物で、用途ごとに3つのライブラリを作成したようです。  

# ライブラリの違い
`big.js`、`bignumber.js`、`decimal.js` のライブラリの違いについて記述していきます。  
各ライブラリがどのように異なるのかについては2015年あたりから常にユーザーの疑問だったようです。[当時のIssue](https://github.com/MikeMcl/big.js/issues/45)ではそれぞれの違いについて質問が寄せられ、結果的に[違いについて記述されたwiki](https://github.com/MikeMcl/big.js/wiki)が作成されました。  
それぞれの違いの詳細はwikiで確認できます。  

https://github.com/MikeMcl/big.js/wiki

## big.js
https://github.com/MikeMcl/big.js
npm trendsでは一番使用されていました。  
また[TypeScriptDeepDive](https://typescript-jp.gitbook.io/deep-dive/recap/number#big.js)でも財務計算の場合はbig.jsのようなライブラリを使用するように書かれています。  

### 特徴
* シンプルなAPI
* JavaのBigDecimalのJavaScriptバージョンよりも速く、小さく、使いやすい
* 容量は6KBとサイズが小さい
* 10進浮動小数点形式で値を格納する
* 依存関係なし
* ECMAScript3のみを使用するため、すべてのブラウザで機能する


## bignumber.js
https://github.com/MikeMcl/bignumber.js/
### 特徴
* 整数と小数に対応
* フル機能のシンプルなAPIが使える
* JavaのBigDecimalのJavaScriptバージョンよりも速く、小さく、おそらく使いやすい
* 圧縮時の容量は8KB
* 暗号的に安全な疑似乱数生成をサポートする
* 依存関係なし
* JavaScript 1.5（ECMAScript 3）機能のみを使用するため幅広いプラットフォームの互換性がある

## decimal.js
https://github.com/MikeMcl/decimal.js/
### 特徴
* 整数と浮動小数点数に対応
* フル機能のシンプルなAPIが使える
* 2進数、8進数、16進数の値も処理できる
* JavaのBigDecimalのJavaScriptバージョンよりも速く、小さく、おそらく使いやすい
* 依存関係なし
* JavaScript 1.5（ECMAScript 3）機能のみを使用するため幅広いプラットフォームの互換性がある
* [math.js](https://github.com/josdejong/mathjs)が内部で使用される
* TypeScript宣言ファイルが含まれる(decimal.d.ts)

# 所感
基本的には `bignumber.js` を使用するのでいいのかなと思います。容量はbig.jsよりは大きいものの展開時19KBほどなので、導入を懸念するほどではなさそうです。  
簡単な計算しか行わないのであれば `big.js`、三角関数を使うような複雑な計算をするなら `decimal.js` という使い分けでしょうか。

そもそも金額の計算とか怖いのでJavaScriptでやりたくないです。。。  


# 参考記事
https://www.mussyu1204.com/wordpress/it/?p=1541#:~:text=%E7%B0%A1%E5%8D%98%E3%81%AB%E8%AA%AC%E6%98%8E%E3%81%99%E3%82%8B%E3%81%A8%E3%80%81javascript,%E3%81%AE%E5%8E%9F%E5%9B%A0%E3%81%AB%E3%81%AA%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82
https://www.cc.kyoto-su.ac.jp/~yamada/programming/float.html
https://ja.wikipedia.org/wiki/IEEE_754
http://blog.asial.co.jp/1191
https://www.macnica.co.jp/business/semiconductor/articles/intel/133327/
https://blog.apar.jp/program/8900/
