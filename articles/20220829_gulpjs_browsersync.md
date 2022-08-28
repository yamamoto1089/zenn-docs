---
title: "Gulp.js+Browsersyncでシンプルなマークアップ環境構築"
emoji: "🥤"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['css','html','gulp','browsersync']
published: true
---

# 記事の内容
HTML,CSS(SCSS)を用いたシンプルなマークアップ用の環境構築の一例  

# やりたいこと
* ReactやVueを使用しないマークアップの環境構築
* インデントなどの設定を開発メンバーで共有したい
* HTML,CSSの修正をリアルタイムで同期させたい

# 環境構築で設定するもの
* node.jsのインストール
* .gitignoreの設定
* gulpfileの設定
* editorconfigの設定
https://ics.media/entry/3405/

# 手順

## node.jsインストール
node.jsのインストールに関しては調べれば色々記事があるので割愛します。  
個人的にはバージョン管理ツールとしてvoltaを入れておくことをお勧めします。　　
https://zenn.dev/taichifukumoto/articles/how-to-use-volta  

既にvoltaをインストールしている場合は以下で最新のLTS版をインストール
```
volta install node
```

## .editorconfigの設定
ルート直下に[.editorconfig](https://editorconfig.org/)を設定することで、異なるエディタ・IDE間でも一貫したコーディングスタイルを定義、維持できます。  
プロジェクトのコーディングルールに合わせて設定すれば、gitで管理している以上、メンバー間でコーディングスタイルを統一できます。  
``` .editorconfig
root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 2
indent_style = tab
max_line_length = 120
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
max_line_length = 0
trim_trailing_whitespace = false
```

## .gitignoreの設定
node_modulesや.vscodeはgitにあげたくないので、忘れないうちにルート直下に `.gitignore` の設定をしておきます。  
``` .gitignore
node_modules
.vscode
```


## gulp.jsとbrowser-syncのインストール
タスクランナーとして `gulp.js` をインストールします。  
```
yarn add -D browser-sync
or
npm install -D browser-sync
```
`browser-sync` も `gulp.js` と連携させて使用するため、先にインストールしておきます。

```
yarn add -D gulp
or
npm install -D gulp
```

## gulpfile.jsの設定
gulp.jsの設定をgulpfile.jsで行います。　　
browserSyncの連携やタスクの設定を記述します。

```js :gulpfile.js
var gulp = require('gulp');
var browserSync = require('browser-sync').create();
var connectSSI  = require('connect-ssi');

gulp.task('default', function() {
  browserSync.init({
    watchOptions: {
      ignoreInitial: true,
      ignored: [ '**/*.+(jpg|jpeg|png|gif|svg)', 'scss', 'node_modules']
    },
    middleware: [
      connectSSI({
        baseDir: './',
        ext: '.html'
      })
    ],
    server: "./"
  });
});
```

## package.jsonにscript追記
package.jsonにgulpを実行するスクリプトを追記します。
```json :package.json
"scripts": {
    "start": "gulp"
  },
```

## 環境立ち上げる
以下コマンド実行でBrowsersyncを実行します。  
```
yarn start
or
npm run start
```
BrowserSyncを実行するとコンソールに以下のようなログが出ます。
```
[Browsersync] Access URLs:
 --------------------------------------
       Local: http://localhost:3000
    External: http://192.168.2.114:3000
 --------------------------------------
          UI: http://localhost:3001
 UI External: http://localhost:3001
 --------------------------------------
```
ExternalのURLが立ち上がったローカルサーバーのURLになります。  

# 感想
HTML,CSS(SCSS)を用いたシンプルなマークアップ用の環境構築の一例としてGulp.jsを用いましたが他にもGruntを使用する方法など、やり方は様々です。  

モジュールバンドラー(webpackなど)を使用するとなると、設定まわりの学習コストがかかったりするので、プロジェクトの規模や目的によって適切なタスクランナーやもクールバンドルの選定が必要になります。  
そのためにもいつくかは自分で環境構築を行なって導入の感覚を掴んで見たいと思いました。  

# 参考記事
https://qiita.com/naru0504/items/82f09881abaf3f4dc171
https://ics.media/entry/3405/
https://ics.media/entry/3405/
https://zenn.dev/yuki0410/articles/8ee3433cfa5b807968d0-2 
https://note.com/kei_01011/n/n34220e142583
