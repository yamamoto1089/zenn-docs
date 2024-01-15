---
title: "license-checker-rseidelsohnを使って正確なパッケージバージョン一覧を取得する"
emoji: "🔏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["yarn", "npm"]
published: false
---

# はじめに

プロジェクトで使用しているパッケージの正確なバージョンを一覧化したい時ってありますよね（？）  
そういうことがあったのでほぼ備忘録的に記事にします。

ついつい `package.json` に記載されてるバージョンがパッケージのバージョンと思ってしまいがちですが、`package.json` に記載されているバージョンはバージョンの範囲を示しているだけで、実際に使用しているバージョンとは異なります。
正確なバージョンは `yarn.lock` や `package-lock.json` に記載されているのでそこを参照する必要がありますが、1 つ 1 つ確認するのは大変です。
一覧で取得したい場合は[license-checker-rseidelsohn](https://github.com/RSeidelsohn/license-checker-rseidelsohn)を使うと以下のコマンドで取得できます。  
※自分は `yarn` を使用しているので `yarn` での使用方法を記載します。

# とりあえず結果から

license-checker-rseidelsohn を devDependencies に追加

```zsh
yarn add -D license-checker-rseidelsohn
```

jq が入っていない場合はインストール

```zsh
brew install jq
```

`license-checker-rseidelsohn` をインストールした前提以下でコマンド実行すると `yarn.lock` に記載されているパッケージの正確なバージョン一覧が出力されます。

```zsh
yarn license-checker-rseidelsohn --product --direct 0 --json | awk '/^{/,/^}/' | jq 'keys'
```

# やってること

## license-checker は使えなかった

license-checker との違い

## 備考

global install すればいいと思ったが推奨されていない

# 参考記事

https://zenn.dev/luvmini511/articles/56bf98f0d398a5
