---
title: "console.log()はオブジェクト参照って話"
emoji: "🪶"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['javascript','console','devTools']
published: false
---

# はじめに

タイトルの通りconsole.log()はオブジェクト参照なので呼び出し時点でのログが出ない時がありいます。console.log()のdocsにも記載されてます。    

> Chrome や Firefox の比較的新しいバージョンを使っているなら注意が必要です。これらのブラウザーで記録されるのはオブジェクトへの参照です。そのため、 console.log() を呼び出した時点でのオブジェクトの「値」が表示されるのではなく、内容を見るために開いた時点での値が表示されます。

呼び出し時点でのログを出したい場合の対処法などについて記載します。  

# 普通に試す
実際に試してみます。
![](https://storage.googleapis.com/zenn-user-upload/fa31956f0b0f-20221103.png)  

firstNameをyamamotoに変更する処理を挟みましたが、1回目と2回目のどちらもfirstNameはyamamotoになっています。  



# 試してみる



# 参考文献
https://ja.javascript.info/object-copy
https://zenn.dev/mryhryki/articles/2020-10-19-hatena-javascript-console
https://zenn.dev/izuchy/articles/0288c3c3a5af1b0da1a3
https://ja.javascript.info/object-copy
