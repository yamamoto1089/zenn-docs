---
title: "console.log()はオブジェクト参照という話"
emoji: "🪶"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['javascript','console','devTools']
published: true
---

# はじめに

人によってはすごく当たり前のことかもしれませんが、タイトルの通りconsole.log()はオブジェクト参照です。なので参照元の値が変化すれば出力される値も変化します。  

> Chrome や Firefox の比較的新しいバージョンを使っているなら注意が必要です。これらのブラウザーで記録されるのはオブジェクトへの参照です。そのため、 console.log() を呼び出した時点でのオブジェクトの「値」が表示されるのではなく、内容を見るために開いた時点での値が表示されます。  

引用: https://developer.mozilla.org/ja/docs/Web/API/console/log#%E5%BC%95%E6%95%B0

# 試してみる

実際にオブジェクト参照になる挙動や対処法を試してみます。  
## オブジェクト参照の出力

オブジェクト参照か試してみます。  

![](https://storage.googleapis.com/zenn-user-upload/fa31956f0b0f-20221103.png)  

1回目出力と2回目出力の間に**firstName**を**yamamoto**に変更する処理を挟みましたが、どちらも**firstName**は**yamamoto**になっていますね。  

でもログ出力したい場合はオブジェクト参照ではなくプリミティブな値としてログを出したい場合の方が多いはずです。  
今回の例だとconsole.logで出力した時点での値を出力するようにします。  

## JSON.parse(JSON.stringify())を使用する

オブジェクト参照ではなくプリミティブな値として出力したい場合は `JSON.perse()` と `JSON.stringify()` を使うようにMDNに記載されています。  

> console.log(obj) を使わず、 console.log(JSON.parse(JSON.stringify(obj))) を使用してください。  
> 
引用: https://developer.mozilla.org/ja/docs/Web/API/console/log#%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E3%83%AD%E3%82%B0%E5%87%BA%E5%8A%9B

![](https://storage.googleapis.com/zenn-user-upload/e18ed4fb11a7-20221110.png)

## console.table()を使用する

そもそもconsole.log()がオブジェクト参照なので、console.log()以外のconsole関数でログを出せば良いです。  
ネストが深くなければconsole.table()で出力すれば良いきがします。  
若干見にくさはありますが。。。

![](https://storage.googleapis.com/zenn-user-upload/f7b820f1e91f-20221110.png)

# おわりに
console.log()はオブジェクト参照であることを知ってなければ変なところで詰まりそうですね。  
そもそもconsoleのメソッド使いこなせてないので[consoleのMDN](https://developer.mozilla.org/ja/docs/Web/API/console)ちゃんと見よ。。。
# 参考文献
https://ja.javascript.info/object-copy  
https://zenn.dev/mryhryki/articles/2020-10-19-hatena-javascript-console  
https://zenn.dev/izuchy/articles/0288c3c3a5af1b0da1a3  

