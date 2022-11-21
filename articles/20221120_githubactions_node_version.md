---
title: "GithubActionsのnodeバージョンをpackage.jsonから取得する"
emoji: "🐙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['GithubActions','volta','package.json']
published: true
---

# はじめに

GithubActionsの**setup-node v3.5.0**で、nodeバージョンをpackage.jsonから取得できるようになりました。  
しかも[voltaやenginesにも対応済み](https://github.com/actions/setup-node/pull/532)なので嬉しいです。  

# 何が嬉しいの？

従来は以下のようにGithubActionsのymlファイルでnode-versionで指定する必要がありました。

```yml
steps:
- uses: actions/checkout@v3
- uses: actions/setup-node@v3
  with:
    node-version: 16
- run: npm ci
- run: npm test
```

プロジェクトのnodeバージョンを上げたタイミングでGithubActionsのnodeバージョンも上げたいですが、その都度node-versionを変更するのは手間です。  
大きいプロジェクトだと使用しているGithubActionsのファイル数も多く、修正もれが出てくる可能性も高くなります。  

その点、package.jsonからnodeバージョンを取得できれば一括で管理できるので、GithubActionsのnode-versionを意識しなくてよくなります。  

# 使い方

**node-version**の代わりに**node-version-file**を指定して、読み込みたいファイルを指定す流だけです。  

```yml
steps:
- uses: actions/checkout@v3
- uses: actions/setup-node@v3
  with:
    node-version-file: 'package.json'
- run: npm ci
- run: npm test
```

# tips

## node-versionとnode-version-fileが両方記載されている場合

この場合は**node-versionが優先**されます。  
https://github.com/actions/setup-node#supported-version-syntax

## voltaとenginesが両方記載されている場合
[voltaとenginesに対応](https://github.com/actions/setup-node/blob/main/docs/advanced-usage.md#node-version-file)していますが、両方書かれている場合は**voltaが優先**されるようなので注意が必要です。  

例えばpackage.jsonが以下の場合はenginesが先に記載されていますが、voltaのnodeバージョンが優先されます。

```json package.json
{
  "engines": {
    "node": ">=16.0.0"
  },
  "volta": {
    "node": "16.0.0"
  }
}
```

## node-version-fileについて
node-version-fileは元々、プロジェクトのNode.jsのバージョン管理しているファイルへのパスを記載すれば、そのファイルからnodeのバージョンを取得できます。  
なのでnvmを使用している場合は以下の指定でnodeバージョンを指定できます。  
```yml
node-version-file: '.nvmrc'
```

# おわりに
GithubActionsで微妙に歯痒かったnode-versionがこれで一括管理できるようになって便利になりました！


過去にはvoltaのバージョンからnodeバージョンを引っ張ってくるアクションを作成された方もいるくらいなので、本当に便利になりました。

https://qiita.com/_kt15_/items/b567a7faf79a183d6d31


# 参考記事
https://blog.lacolaco.net/2022/10/github-actions-setup-node-engines/　　
https://zenn.dev/yassh/articles/5a9c761c10aaed
