タイトル
いつの間にかインストールされてた「-」というnpmパッケージについて

# はじめに

:::note info
[TLB Enjoy Developers Advent Calendar 2022](https://qiita.com/advent-calendar/2022/tlb-enjoy) の6日目の記事です。
:::

いつもはAdventCalendarは見る側だったのですが、今回は初めて記事を書く側として参加できて嬉しいです！  
**TLB Enjoy Developers Advent Calendar 2022** はテーマなど制限がないので自由に書こうと思います！  

今回の記事ではnpmパッケージ「-」についてです！  
特に技術的な記事ではなく、調べてみた系の記事ですが読んでもらえると嬉しいです。  

# 経緯

## 「-」を知るきっかけ

ある日 **package.json** の **dependencies** を見ていると、ふと気になるパッケージがありました。  

```json
  "dependencies": {
    "-": "^0.0.1",
  }
```

`"-": "^0.0.1",` という見覚えのないパッケージの記載がありました。  
しかも **dependencies** に...

## 「-」について調べる

「"-"なんてパッケージ入れた覚えはないぞ!」と思ってnpmで調べてみると、「-」って名前のパッケージは確かにありました。  
![スクリーンショット 2022-12-06 4.50.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/506401/2b555bd3-4f16-74df-0a7e-65b5ae498dcd.png)
引用: https://www.npmjs.com/package/-?activeTab=readme  

2020年4月2日にバージョン0.0.1が登録されており、そこから全く更新されていません。  
ファイル数も3ファイルで600Bしかありません。  
GitHubで[「-」のリポジトリ](https://github.com/parzhitsky/-  )の中身を見てみてもパッケージ作成に最低限必要そうなファイルがあるだけで、特に意味のあるコードは記載されていません。  

その割にWeeklyDownloadsは **38,087** とそこそこダウンロードされています。  
「-」はなんのために作成されたパッケージなのか、なぜ自分の **package.json** に記述があったのか謎が深まるばかりです。  
他の記事も参考にしながら調べていくと色々と分かったので記載していきます。  


# 結局npmパッケージ「-」とはなんなのか

2021年8月のこちらの記事に疑問の全て書かれていました。  
https://www.bleepingcomputer.com/news/software/empty-npm-package-has-over-700-000-downloads-heres-why/

日本語訳された記事はこちら　　
https://postd.cc/empty-npm-package-has-over-700-000-downloads-heres-why/

## なぜ「-」が作成されたか

> 最初は、'-' が命名規則に従って有効なパッケージ名であることを確認するためにパッケージを公開しました。

引用(Google翻訳): https://www.bleepingcomputer.com/news/software/empty-npm-package-has-over-700-000-downloads-heres-why/

とのことで、そもそもは「-」の作成者の[Parzhitsky](https://github.com/parzhitsky)さんが、[npmの命名規則](https://github.com/npm/validate-npm-package-name#naming-rules)に則った「-」を実際に公開できるのか試したとことから公開されたようです。  

## 「-」は安全か

「-」が個人の作成したパッケージである以上、悪意のあるコードが埋め込まれることも考えられます。  
これについても制作者が悪用する意図はないことを言っています。  

## なぜ「-」がインストールされたか

「-」がインストールされた原因は **タイピングミス** でした。  

パッケージをインストールする際は
```shell
yarn add -D {package}
```
or
```shell
npm install -D {package}
```
などでインストールしますし、オプションをつけることが多いです。  

この時、パッケージをスペース区切りで複数指定することで、まとめてインストールすることができます。  

つまり、以下のコマンドを実行してしまうと、指定したパッケージと一緒に「-」や「D」がインストールされるわけです。  
※「D」というパッケージも存在します

```shell
yarn add - D　{package}
```
or
```
npm install - D {package}
```

普通は気付きそうですが、脳死でインストールなどを行なっていると気がつかないうちに「-」がインストールされるわけですね。  


# 「-」以外のものも試してみる
調べてみたら「-」や「D」以外にも[よく使いそうなオプション](https://qiita.com/rubytomato@github/items/1696530bb9fd59aa28d8#%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89)と同じ名前のパッケージがあるようです。  
せっかくなので(?)インストールしてみます。  


## オプションと同じ名前でインストールできたパッケージ

よく使うオプションの名前でインストールを試して、存在したものだけを記載しました！  
以下のコマンドで一気にインストールできます！

```shell
yarn add - D O g i exact tag tilde version
```
or
```shell
npm install - D O g i exact tag tilde version
```

nnpmtrendsでみると、どれもインストールされてそうですね！  
（中にはちゃんと意味のあるパッケージもありそう。）

https://npmtrends.com/--vs-D-vs-O-vs-exact-vs-g-vs-i-vs-tag-vs-tilde-vs-version


**package.json** のなか↓
```json
  "dependencies": {
    "-": "^0.0.1",
    "D": "^1.0.0",
    "O": "^0.0.9",
    "exact": "^0.8.0",
    "g": "^2.0.1",
    "i": "^0.3.7",
    "tag": "^0.4.17",
    "tilde": "^0.1.1",
    "version": "^0.1.2",
  },
```

**なんか満足！！！**  

# おわりに
オプションをつけてインストールする場合は注意が必要が必要なことや、確認することの重要性を「-」から学びました！  
今回記事で紹介したような意味のないパッケージが他にもあれば教えて欲しいです！  

明日は @Yu_yukk_Y さんの記事です！お楽しみに！

# 参考記事
https://www.bleepingcomputer.com/news/software/empty-npm-package-has-over-700-000-downloads-heres-why/  
https://postd.cc/empty-npm-package-has-over-700-000-downloads-heres-why/
