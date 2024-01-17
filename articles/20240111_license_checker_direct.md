---
title: "yarn.lockに記載されているパッケージバージョン一覧を取得する"
emoji: "🔏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["yarn", "npm"]
published: false
---

# はじめに

プロジェクトで使用しているパッケージの正確なバージョンを一覧化したい時ってありますよね（？）  
そういうことがあったので備忘録的に記事にします。

ついつい `package.json` に記載されてるバージョンがパッケージのバージョンと思ってしまいがちですが、`package.json` に記載されているバージョンは範囲を示しているだけで、実際に使用しているバージョンとは異なります。
正確なバージョンは `yarn.lock` や `package-lock.json` に記載されているのでそこを参照する必要がありますが依存関係も記載されており 1 つ 1 つ確認するのは大変です。  
パッケージのライセンスを確認する `license-checker-rseidelsohn` を使えば比較的簡単に一覧かできました。

# とりあえず結果から

license-checker-rseidelsohn をインストール  
（global に追加してますが、yarn 2 系であれば dlx で追加したいです）

```zsh
yarn global add license-checker-rseidelsohn
```

[jq](https://github.com/jqlang/jq) が入っていない場合はインストール

```zsh
brew install jq
```

コマンド実行

```zsh
yarn license-checker-rseidelsohn --production --direct 0 --json | awk '/^{/,/^}/' | jq 'keys'
```

出力結果

```zsh
[
  "-@0.0.1",
  "D@1.0.0",
  "O@0.0.9",
  "exact@0.8.0",
  "g@2.0.1",
  "i@0.3.7",
  "optional@0.1.4",
  "tag@0.4.17",
  "tilde@0.1.1",
  "version@0.1.2",
  "zenn-cli@0.1.150"
]
```

# コマンド内でやってること

オプションについては license-checker-rseidelsohn の[README](https://github.com/RSeidelsohn/license-checker-rseidelsohn)に記載されてますが簡単に書いていきます。

## --production

`--production` は devDependencies に記載されているパッケージを除外するオプションです。  
`--production` を指定しないと devDependencies に記載されているパッケージも一覧に含まれます。
逆に devDependencies に記載されているパッケージが欲しい場合は `--development` を指定します。

## --direct 0

`--direct [boolean|number]` というオプションで、どこまでの依存関係を含めるかを指定しています。

README には`--direct true` の指定で直接の依存関係を出力すると記載されていますが、実際には、このオプションを使うと依存関係を含んだ出力になるので注意です。
なのでここでは `--direct 0` として、０階層（実質的に直接的な依存関係）に該当するパッケージのみを出力するようにしています。

## --json

そのままです。json 形式で出力するオプションです。

## awk '/^{/,/^}/'

[awk コマンド](https://www.ibm.com/docs/ja/aix/7.2?topic=awk-command) で 正規表現を用いて json 形式の出力の `{` から `}` までの部分を抽出しています。
`yarn` の設定で `yarn run` のログなどが出力される設定だと、以下のように余分なログが出力されてしまうので、それを除外するために使用しています。

```zsh
yarn run v1.22.19
$ {path}/node_modules/.bin/license-checker-rseidelsohn --production --direct 0 --json
{
  "hoge": {
    "fuga": "piyo"
  }
}

✨  Done in 0.64s.
```

一応 yarn 自体の設定でログを出力しないようにもできます。  
参考: https://sunday-morning.app/posts/2021-05-11-yarn-run-silent

## jq 'keys'

`jq 'keys'` コマンドで json 形式の出力からキーの一覧を取得しています。  
今回の場合、キーの `パッケージ名@バージョン` の情報だけが欲しかったので他の情報を除外するために使用しました。

```json
    "zenn-cli@0.1.150": {
        "licenses": "MIT",
        "repository": "https://github.com/zenn-dev/zenn-editor",
        "path": "/Users/yamamotonaoyuki/private/zenn-docs/node_modules/zenn-cli",
        "licenseFile": "/Users/yamamotonaoyuki/private/zenn-docs/node_modules/zenn-cli/LICENSE"
    },
```

# 余談

## license-checker ではなくて license-checker-rseidelsohn を使った理由

できれば [license-checker](https://github.com/davglass/license-checker#readme) を使いたかったですが、直接の依存関係があるものだけを出力する`--direct` オプションが動作しない問題がありました。（issue に挙げられているものの解決しないまま）  
参考: https://github.com/davglass/license-checker/issues/191

なので`license-checker`の代用として license-checker からフォークして作られた license-checker-rseidelsohn を使用しました。

## yarn 2 系であれば license-checker-rseidelsohn は yarn dlx で実行したい

`license-checker-rseidelsohn`はパッケージバージョンを出力するためだけに使用したいので、プロジェクトに依存しない`yarn dlx`でインストールしたいです。

1 系であれば global でインストールできますが、yarn の思想的に今後は推奨されなくなるようです。

> Use yarn dlx instead of yarn global
> Yarn focuses on project management, and managing system-wide packages was deemed to be outside of our scope. As a result, yarn global got removed and needs to be replaced by yarn dlx to run one off scripts.

参考: https://yarnpkg.com/migration/guide#use-yarn-dlx-instead-of-yarn-global

# 参考記事

https://zenn.dev/luvmini511/articles/56bf98f0d398a5
https://qiita.com/ssc-ynakamura/items/90c6fe31b5f6fe0989ac
