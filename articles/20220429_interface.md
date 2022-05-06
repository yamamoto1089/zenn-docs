---
title: "型定義ファイルのないライブラリのインポートエラーについて"
emoji: "🎛"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['TypeScript','JavaScript']
published: true
---

# はじめに
TypeScriptを使用したプロジェクトで、型定義ファイルがないJavaScriptライブラリを使用すると以下のような警告が出ます。  

![](https://storage.googleapis.com/zenn-user-upload/40f8d7580f4b-20220506.png)

```
モジュール '{モジュール名}' の宣言ファイルが見つかりませんでした。'{モジュール名}' は暗黙的に 'any' 型になります。
存在する場合は `npm i --save-dev @types/{モジュール名}` を試すか、`declare module '{モジュール名}';` を含む新しい宣言 (.d.ts) ファイルを追加します
```

警告の対処法と、そもそも `d.ts` ファイルがなんなのかについて記していきます。

# 結論
対処法の結論を先に記載すると、警告文にあるように、`declare module '{モジュール名}'; を含む新しい宣言 (.d.ts) ファイルを追加` することで解決できます。  

実例を参考に手順を説明していきます。

今回、types対応していないJavaScriptライブラリとして[Gio.js](https://zenn.dev/ymmt1089/articles/20220401_giojs)を使用しており、`Gio.js` のインポート節でエラーが出ていました。

```ts: index.tsx
import * as GIO from "giojs";
```
<!-- textlint-disable  -->
`Gio.js` はTypeScript用の型定義ファイルが用意されていないので、エラー文にある
```
npm i --save-dev @types/{モジュール名}
```
 は使用できません。
<!-- textlint-enable  -->

よって `declare module '{モジュール名}'; を含む新しい宣言 (.d.ts) ファイルを追加` を試します。  

任意のディレクトリ階層に以下の `giojs.d.ts` を新規作成しました。  

```d.ts: giojs.d.ts
declare module "giojs";
```

これでエラーは出なくなります。

# そもそもd.tsファイルって何？

詳しくはサバイバルTypeScriptで解説されていますが、`d.ts` ファイルは型定義ファイルのことを指します。

https://typescriptbook.jp/reference/declaration-file

型定義ファイル(.d.ts)にはアクセス可能な型宣言を記述します。  
JavaScriptライブラリには型情報がないため、TypeScriptのプロジェクトで使用するときは、型定義ファイルが必要ということですね。  
ただし、この型定義ファイルはすべてのライブラリに存在するわけではないため今回のように自分で仮定義する必要があります。

# 型定義ファイルの有無による作業の違い
今回は型定義ファイルを作成することで対応しましたが、元々型定義ファイルが存在する場合など、型定義ファイルの有無によって作業が変わってきます。  

## [型定義ファイル有りの場合](https://typescriptbook.jp/reference/declaration-file#%E5%9E%8B%E5%AE%9A%E7%BE%A9%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E6%9C%89%E3%82%8A)
型定義ファイルが含まれているパッケージの場合、特別な作業は必要ありません。  
そのままインポートを行えば良いです。
```
npm install {ライブラリ名}
```

## [型定義ファイル有りだが別途インストールが必要な場合](https://typescriptbook.jp/reference/declaration-file#%E5%9E%8B%E5%AE%9A%E7%BE%A9%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E6%9C%89%E3%82%8A%E3%81%A0%E3%81%8C%E5%88%A5%E9%80%94%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%8C%E5%BF%85%E8%A6%81)
ライブラリに型定義ファイルが同梱されていない場合は別途インストールする必要があります。  
通常通りライブラリをインポートした後に `@types` で型定義ファイルをインポートすれば良いです。  

※今回のエラー文の最初に提示されていた解決法はこれに該当します。
```
npm install {ライブラリ名} --save # express本体のインストール
npm install @types/{ライブラリ名} --save-dev # 型定義ファイルのインストール
```

## [型定義ファイル無し](https://typescriptbook.jp/reference/declaration-file#%E5%9E%8B%E5%AE%9A%E7%BE%A9%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E7%84%A1%E3%81%97)
型定義ファイルがないライブラリなので、以下のどちらかで対処できます。
* `any` 型で妥協する
* 型定義ファイルを作る

### any型で妥協する
これは今回行った対処法です。  
型定義ファイル自体が存在しないことでエラーが出ていたので、`giojs.d.ts` を作成しました。  
型定義ファイルを作成したのでエラー自体は出なくなりましたが、詳細な型定義がされていないので暗黙的に `any` 型になります。  

### 型定義ファイルを作る
型定義ファイルを自作して[DefinitelyTyped](http://definitelytyped.org/guides/contributing.html)に公開する対処法です。  
かなり時間と労力を使う対処法にはなります。


[DefinitelyTypedの日本語READ.ME](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/README.ja.md)


# おわりに
案件で扱うようなライブラリは大体型定義ファイルに対応していることが多いので、まずは `@types` を試してみることをお勧めします。

時間に余裕がなければ、型定義ファイルがない場合は `.d.ts` ファイルで仮の型定義ファイルを作成し `any` 型を許容するしかない気もします。 　
型安全を担保できなくなるので、トレードオフな気もしますが。

# 参考記事
https://typescriptbook.jp/reference/declaration-file
https://bnsgt.hatenablog.com/entry/2021/12/24/193825
https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/README.ja.md
