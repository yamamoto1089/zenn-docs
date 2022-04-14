---
title: "textlintのエラーをGithubActionsで自動修正してみた"
emoji: "🖋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["markdown","textlint","GithubActions"]
published: false
---

# はじめに

以前投稿した記事、「Github連携されたZennの記事をtextlintで自動チェックする」の続きになります。  
https://zenn.dev/ymmt1089/articles/c81d1174e0197e 

以前投稿した記事、「Github連携されたZennの記事をtextlintで自動チェックする」ではGithubActionsでtextlintを走らせるだけでした。  
しかし、textlintでエラーが出た場合、手動で修正し再びプッシュするのは面倒です。  
textlintは自動修正機能がありますので、Githubにプッシュしたタイミングで自動修正し、修正結果を自動でマージする方法を共有します。  

# 前提

[以前の記事](https://zenn.dev/ymmt1089/articles/c81d1174e0197e)の続きなので、Githubの記述や基本的な設定は前回の記事を参考にしてください。  

# 今回できるようになること
例えば以下のような文章をGithubにプッシュすると。

GithubActions上でtextlintの自動修正が実行され。

ブランチにプッシュしてくれます。

結果、以下のように自動修正されたものがブランチにマージされました。
