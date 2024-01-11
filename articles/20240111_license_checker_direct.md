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

#

ついつい package.json に記載されてるバージョンがパッケージのバージョンと思ってしまいがちですが、package.json に記載されているバージョンはバージョンの範囲を示しているだけで、実際に使用しているバージョンとは異なります。
正確なバージョンは yarn.lock や package-lock.json に記載されているのでそこを参照する必要がありますが一つ一つ確認するのは大変です。
一覧で取得したい場合は license-checker-rseidelsohn を使うと簡単に取得できます。

# やっていること

## license-checker は使えなかった

license-checker との違い

## 備考

global install すればいいと思ったが推奨されていない

# 参考記事

https://zenn.dev/luvmini511/articles/56bf98f0d398a5
