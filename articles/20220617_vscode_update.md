---
title: "VSCodeを自分仕様にしていく"
emoji: "⚙️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["vscode","tips"]
published: true
---

# はじめに
開発ではVisualStudioCodeをよく使用します。  
開発効率を上げるために行なった、VSCodeの設定変更や拡張機能について備忘録を残す目的です。  

# 目次
* フォントをJetBrainsMonoに変更する
* サイドバーの位置を右に変更する
* 言語モードの変更をショートカットに割り当てる

## フォントをJetBrainsMonoに変更する
JetBrainsのIDEで使用されている[JetBrainsMono](https://www.jetbrains.com/ja-jp/lp/mono/)に変更します。  
“1”、“l”、“I”や“0”、“O”などの文字を識別しやすくしていたり、シンプルな書式にすることで可読性を上げたフォントなので良いです。  

### フォントのダウンロード
https://www.jetbrains.com/ja-jp/lp/mono/#ligatures
JetBrainsのサイトからJetBrainsMonoをダウンロードします。  
`JetBrainsMono.zip` を解凍し、`fonts` -> `ttf` から好きなフォントを選択しインストールします。  
基本的には以下の3つをインストールすれば十分かと思います。

* `JetBrainsMono-Regular.tff`
* `JetBrainsMonoNL-Italic.ttf`
* `JetBrainsMonoNL-Bold.ttf`

### VSCodeのフォント指定
VScodeの設定を開き、検索ボックスで `font family` を検索します。  
画像の赤枠部分のように `Editor:Font Family` の項目があるので、そこに `JetBrains Mono` を追加します。  
![](https://storage.googleapis.com/zenn-user-upload/f19ab960621e-20220624.png)

左に記載されているフォントが優先されるので、一番左に記載します。  
あとはVSCodeを再起動すればJetBrainsMonoのフォントで表示されます。

## サイドバーの位置を右に変更する
通常サイドバーは左に配置されていますが、これを右に変更します。  
サイドバーを表示/非表示(`command+B`)した際に、読んでいるファイルの位置が右にずれることを防ぎたいためです。

* サイドバーを左に表示した場合(通常状態)
  開いているファイルがサイドバー分右にずれて読みにくい
![](https://storage.googleapis.com/zenn-user-upload/a6356dfa39f3-20220624.gif)

* サイドバーを右に表示した場合
  開いているファイルがずれることがなく読みやすい
![](https://storage.googleapis.com/zenn-user-upload/268f67929ffc-20220624.gif)

### サイドバーなどのレイアウトの変更
画面右上の `レイアウトのカスタマイズ` のアイコンを押します。
![](https://storage.googleapis.com/zenn-user-upload/851c9fc461a6-20220624.png)

`プライマリサイドバーの位置` を右に変更します。
![](https://storage.googleapis.com/zenn-user-upload/477e7e02673c-20220624.png)

他にもレイアウトが変更したい場合は `レイアウトのカスタマイズ` から変更できます。

## 言語モードの変更をショートカットに割り当てる
apiから返却されたjsonデータとかをフォーマットして見やすくしたい場面が稀にあります。
しかし保存していないファイルをフォーマットしたい場合は、言語モードを設定しなければうまくフォーマットが走りません。  

例えば以下のデータをjson形式にフォーマットしたい場合、
```
{key1:'value',key2:'value',key3:'value',key4:'value',key5:'value',key6:'value',key7:'value'}
```
`option + shift + F` でフォーマットを試みると、言語モードが指定されていないのでこのような警告が表示されます。
![](https://storage.googleapis.com/zenn-user-upload/5bc5bfd1bfe3-20220625.png)

いちいち設定から言語モードを設定するのは面倒なのでショートカットキー化します。

### 言語モードのショートカット割り当て方法
`command + shift + P` でコマンドパレットを表示し、「言語モード」と入力します。  
「言語モードの変更」の項目が表示されるので、右側の歯車マークを押します。  
![](https://storage.googleapis.com/zenn-user-upload/6dee3e03633d-20220625.png)

キーボードショートカットの画面が表示されるので、「言語モードの変更」左側の鉛筆マークを押します。
![](https://storage.googleapis.com/zenn-user-upload/08dcced21f5d-20220625.png)

キーバインドの入力画面が表示されるので、他のショートカットと競合しないショートカットを入力します。(自分は `command+shift+I` にしてます)
![](https://storage.googleapis.com/zenn-user-upload/0669b21cce11-20220625.png)

設定は完了です。これでファイルを保存せずにキー入力だけでフォーマットが行える様になりました。  
![](https://storage.googleapis.com/zenn-user-upload/cecf23858d54-20220625.gif)
↑で行なった動作
* command+N (新規ファイル追加)
* command+V (フォーマットしたいテキストをペースト)
* command+shift+I (言語モードの設定開く)
* フォーマットしたい言語形式を入力しEnter
* option+shift+F (フォーマットを走らせる)

# おわりに
「こうした方がいいよ」とか「こんなのもあるよ」とかあれば教えてほしいです🙇‍♂️  
今後も定期的に更新していきます。
