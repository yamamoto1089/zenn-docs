---
title: "Babylon.jsを使ってNextアプリでGiuHubSkylineを表示させたい"
emoji: "🧊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['babylonjs','javascript','next']
published: false
---

# はじめに

Githubのプロフィール画面なんて滅多に開かないんですが、久しぶりに見てみると `Pull Shark` とかいうバッチが追加されてました。  
![](https://storage.googleapis.com/zenn-user-upload/90408d9e6d8a-20220618.png)

最近Githubに[Achievementsのバッチが追加](https://zenn.dev/nyancat/articles/20220612-github-achievements)さたみたいで、Pull Shark`もそのひとつみたいですね。

他にも変わったところないかなーと思ってみてみると、草生やしてるところになんか追加されてました。  
↓草生やしてるところ
![草生やしてるところ](https://storage.googleapis.com/zenn-user-upload/4c48c26d700d-20220618.png)

![NEW! View your contributions in 3D, VR and IRL!](https://storage.googleapis.com/zenn-user-upload/58e706d6be7d-20220618.png)

`NEW! View your contributions in 3D, VR and IRL!`ってなんだと思ってクリックしてみるとカッコよくなった草生やしてるやつが見れました。

GitHubSkylineって名前らしいです。
![](https://storage.googleapis.com/zenn-user-upload/71ebf12fa3d4-20220618.gif)

ダウンロードできるっぽいのでしてみるとslt形式の草生やしてるやつがダウンロードできました。

3Dデータをblender経由でBabylon.jsで表示できるように変換できるものがあるらしいです。
この機会にGitHubSkylineのデータを使って試してみます。  

# 余談
そもそもGitHubSkylineはBabylon.jsで表示されてるみたいですね。
https://forum.babylonjs.com/t/github-skyline/30806

今回自分が行ったのは手順は
* babylon.jsで表示
* .slt形式でダウンロード
* blenderにインポート
* .babylon形式でエクスポート
* Nextアプリ上でbabylon.jsで表示
なので、なんか虚無です。一体何がしたかったのか。


# 参考記事
https://www.crossroad-tech.com/entry/babylonjs-exporter-blender2.80
