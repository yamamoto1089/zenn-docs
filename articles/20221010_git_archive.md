---
title: "Githubで差分ファイル一覧を抽出する"
emoji: "📓"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Github","archive"]
published: true
---

# はじめに
「**作業したファイルをリスト化してほしい**」なんてことを言われる日が来るとは思いませんでした。  
一体そんなものを出してどうするんだ、そもそもその作業はこちらがやらなければいけないのか、手作業では絶対にやりたくない。。。 
そんな思いに駆られながらググっているとGit管理できていればなんとかなるみたいですね。  

それに、作業したファイルをリスト化したいって意外とあるあるみたいですね。備忘録として記事にしました。  
Githubのコマンド一発で差分を抽出しましょう。  

# とりあえず差分を抽出する

**コミットID:A**から**コミットID:B**までの差分を抽出したい場合は以下のコマンドで抽出できます。  
```
git archive {コミットID:A} `git diff --name-only {コミットID:A} {コミットID:B}` -o {出力ファイル名}
```

なので例えば、最新から1つ前の差分を**archive.zip**に出力する場合は以下のコマンドです。
```
git archive HEAD `git diff --name-only HEAD HEAD~2` -o archive.zip
```

## 何をやってるの？

`git diff` を使用して特定のコミットの間で発生した差分を抽出、`git archive` で抽出された差分をアーカイブしています。  

# エラーが出る場合の対処

`git archive` できないファイルがある場合は以下のようなエラーが出ます。
```
fatal: pathspec '{ファイル名}' did not match any files
```

この場合はエラーの原因ファイルを[sedコマンド](https://tech-blog.rakus.co.jp/entry/20211022/sed)で除外します。
```
git archive {コミットID:A} `git diff --name-only {コミットID:A} {コミットID:B}| sed '{除外するファイル}'` -o {出力ファイル名}
```

例えば **.DS_Store** ファイルが原因でエラーが出ている場合は以下のようになります。
```
git archive HEAD `git diff --name-only HEAD HEAD~2| sed '.DS_Store'` -o archive.zip
```

コマンドの併用でだいぶ長くなりましたが、これで目的はほぼ達成できました。  

# 抽出されたファイルのパスをリスト化

抽出されたzipファイルの中身のパスをリスト化します。  
[zipinfo](https://4thsight.xyz/3761)コマンドを使用してzipファイル内のファイル名を1行ずつ出力して保存します。  
```
zipinfo -1 {抽出したzipファイル} > {出力先ファイル}
```
例えば、今回のarchive.zipのファイル名をresult.txtに出力する場合は以下のようになります。
```
zipinfo -1 archive.zip > result.txt
```

**zipinfo**のオプションで **-1** を指定していますが、ここのオプションを変更することで圧縮率やタイムスタンプなど詳細場情報を出色することもできます。  
**-x**を指定すれば特定のファイル名を出力から除外することもできます。  

# 最適化
これでコミットAからコミットBまでの差分のファイルをリスト化できました。  
ただ **.gitignore** など不要なファイルもリスト化されているので、必要に応じて不要ファイルを除外する必要があります。  
**.gitignore**は[.gitattributes](https://zenn.dev/woo_noo/articles/5fcee179d890d1174add)に `.gitignore export-ignore` を記述することでarchiveの対象外にできるみたいです。  

他にも必要に応じて不要なファイルを除外する必要はありそうですが、多くなければ人力でやっても良いのかなと思います。  

# おわりに
「作業したファイルをリスト化してほしい」なんてことを言われた日にはこの記事を思い出してください。  
次はお前の番です。  

# 参考記事
https://qiita.com/kaminaly/items/28f9cb4e680deb700833
https://rfs.jp/server/git/gite-lab/git-archive.html
https://zenn.dev/lamrongol/articles/d21f42c27495b3
