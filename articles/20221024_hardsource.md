---
title: "HardSourceWebpackPluginでエラーが出た場合の対処法"
emoji: "🛠"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['node.js','HardSourceWebpackPlugin','webpack']
published: true
---

# はじめに

ローカルでビルドした時に以下のようなエラーが出ることがあります。  

```
ERROR  [hardsource:dca56a80] Could not freeze {ファイルのパス}: Cannot read properties of undefined (reading 'hash')
```

対処法とそもそも何のエラーなのかを解説していきます。  

# 結論

ひとまずエラーが出ているHardSourceで作成されたキャッシュファイルを削除することで解決できます。  

```shell
$ rm -rf node_modules/.cache/hard-source/
```

# 原因

WebPackプラグインの[HardSourceWebpackPlugin](https://github.com/mzgoddard/hard-source-webpack-plugin)で生成したキャッシュが壊れてエラーを起こしています。  
HardSourceWebpackPlugin自体はキャッシュを生成することで2回目以降のビルドを高速化するプラグインです。  
キャッシュを参考にビルドを高速化しますが、そのキャッシュファイル自体が壊れてしまっているためビルド時にエラーがでていたようです。  

特に設定を変更していなければ `node_modules/.cache/hard-source/` にキャッシュが生成されるので、今回はそのキャッシュファイル自体を削除してしまえば良いということです。  
キャッシュを削除するので、削除した後の1回目のビルドは時間がかかりますが、まあ仕方ないですね。

# 余談

[hardsourceの問題を解決するPR](https://github.com/mzgoddard/hard-source-webpack-plugin/pull/497)が2019年4月時点で挙げられていますが一向にマージされる気配がありません。  
色々調べていくと、そもそもHardSourceWebpackPlugin更新されていないようですね。  

PRには「まだマージされないの？」みたいなコメントがいくつかついており、みんながこの問題を解消するPRがマージされることを心待ちにしてい流ものの、開発者からの音沙汰なしのようです。

そんな中2020年5月30日に開発者の昔の同僚が「どのPRをマージすべきか教えて」みたいな[issue](https://github.com/mzgoddard/hard-source-webpack-plugin/issues/525)を起票しました。  

2020年の8月にはそのissueに対して「このPRはマージしても安全だよ」みたいな[コメント](https://github.com/mzgoddard/hard-source-webpack-plugin/issues/416)が付いていました。  
それでも開発者の昔の同僚からのアクションはなく、いまだにマージされていないようですね。  

個人的な意見ですが、そもそもHardSourceWebpackPluginに頼らずにビルドが重い原因を探して対応することも必要かもしれないですね。  
バンドルサイズがでかい原因やコードをリファクタしてコード数を減らして処理を軽くするとかですかね。  

# 参考記事
https://qiita.com/HorikawaTokiya/items/cfd7a2f1711402831f54
https://zenn.dev/sa2knight/articles/7e4d6b43780d9fc47ab1
https://zenn.dev/naturalclar/articles/4d9a22d786f5cd7903f2
