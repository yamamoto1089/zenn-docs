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

textlintの自動修正に関して、lintエラー全てを修正できるわけではありません。textlint実行時のログにチェックマークがついているエラーだけが自動修正で修正可能です。  
あくまでも補助機能の1つとして利用します。

# 手順
* 自動修正を走らせるコマンド設定
  * package.jsonの修正
* GithubActionsのワークフロー追加  
  * texilintの自動修正実行
  * Githubの設定
  * textlintの自動修正をプッシュ

# 自動修正を走らせるコマンド設定
まずはローカルでもtextlintの自動修正が実行できるようにpackage.jsonにコマンドを追加します。
## package.jsonの修正

textlintで自動修正を行うオプションは標準で用意されています。  
`yarn textlint {ファイル名} --fix` のように末尾に `--fix` のオプションをつけるだけです。  

```diff json:package.json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "textlint": "textlint \"./{articles,books}/*.md\"",
+    "textlint:fix": "yarn textlint --fix"
  },
```

今回は `textlint:fix` の名前でコマンドを追加しました。  
`yarn textlint:fix` を実行すればtextlintの自動修正をおこなってくれます。

# GithubActionsのワークフロー追加

ここからはGithubActionsにプッシュされた後に実行することを設定していきます。  
前回作成した `textlint.yml` に追加していきます。

```yml:textlint.yml
name: textlint
on: push
jobs:
  build:
    name: check textlint
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 16.x
      - run: yarn install
      - name: textlintのエラーを自動修正
        run: yarn run textlint:fix
      - name: Githubの設定
        run: |
          git remote set-url origin https://github-actions:$GITHUB_TOKEN@github.com/$GITHUB_REPOSITORY
          git config --global user.name $GITHUB_ACTOR
          git config --global user.email $GITHUB_ACTOR@user.noreply.github.com
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - name: textlintの自動修正をマージ
        run: |
          git add .;
          git commit -m "🖋textlint auto fixed!!!🖋";
          git push origin HEAD:${GITHUB_REF};

```

## texilintの自動修正実行

プッシュされた時に、先ほど設定した `yarn run textlint:fix` を実行するように修正します。  

```diff yml:textlint.yml
name: textlint
on: push
jobs:
  build:
    name: check textlint
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 16.x
      - run: yarn install
+      - name: textlintのエラーを自動修正
+        run: yarn run textlint:fix
-      - name: Check Textlint
-        run: yarn run textlint

```

## Githubの設定

次にGiuhubActionsからGiuhubにプッシュを行うための設定です。　　
`.gitconfig` のグローバルに設定している `user.name` や `user.email` を設定しています。

```diff yml:textlint.yml
name: textlint
on: push
jobs:
  build:
    name: check textlint
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 16.x
      - run: yarn install
      - name: textlintのエラーを自動修正
        run: yarn run textlint:fix
+      - name: Githubの設定
+        run: |
+          git remote set-url origin https://github-actions:$GITHUB_TOKEN@github.com/$GITHUB_REPOSITORY
+          git config --global user.name $GITHUB_ACTOR
+          git config --global user.email $GITHUB_ACTOR@user.noreply.github.com
+        env:
+          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

## textlintの自動修正をプッシュ

最後に `add`,`commit`,`push` の設定を追加します。

```diff yml:textlint.yml
name: textlint
on: push
jobs:
  build:
    name: check textlint
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 16.x
      - run: yarn install
      - name: textlintのエラーを自動修正
        run: yarn run textlint:fix
      - name: Githubの設定
        run: |
          git remote set-url origin https://github-actions:$GITHUB_TOKEN@github.com/$GITHUB_REPOSITORY
          git config --global user.name $GITHUB_ACTOR
          git config --global user.email $GITHUB_ACTOR@user.noreply.github.com
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
+      - name: textlintの自動修正をプッシュ
+        run: |
+          git add .;
+          git commit -m "🖋textlint auto fixed!!!🖋";
+          git push origin HEAD:${GITHUB_REF};

```

これでtextlintの自動修正をプッシュできるようになりました。

# おわりに
Githubにプッシュしたタイミングでtestlintの自動修正をおこなってくれるのは便利に感じました。  
ただ、textlintの自動修正をおこなっても差分がなかった場合にGithubActionsでエラーになることが課題として残っています。　　
差分がある場合のみプッシュのフローを実行できるように、修正の余地がありそうです。
(参考記事の「GitHub Actionsでワークフロー中に発生した差分をPushする」ではその点も考慮されています。)

# 参考記事

https://zenn.dev/lollipop_onl/articles/eoz-gha-push-diffs 
