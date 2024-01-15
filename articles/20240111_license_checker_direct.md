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
正確なバージョンは `yarn.lock``やpackage-lock.json` に記載されているのでそこを参照する必要がありますが1つ1つ確認するのは大変です。
一覧で取得したい場合は[license-checker-rseidelsohn](https://github.com/RSeidelsohn/license-checker-rseidelsohn)を使うと以下のコマンドで取得できます。

```zsh
yarn license-checker-rseidelsohn --product --direct 0 --json
```

# 結果から

`license-checker-rseidelsohn` をインストールした前提以下でコマンド実行すると`yarn.lock`に記載されているパッケージの正確なバージョン一覧がjsonで取得できます。

```zsh
yarn license-checker-rseidelsohn --product --direct 0 --json
```

# 手順

## license-checker は使えなかった

license-checkerとの違い

## 備考

global installすればいいと思ったが推奨されていない

# 参考記事

https://zenn.dev/luvmini511/articles/56bf98f0d398a5