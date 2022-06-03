---
title: "GenerativeArt界隈について調べてみた"
emoji: "🦆"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ['GenerativeArt','Processing','p5js']
published: false
---

# はじめに
以前[p5jsについて記事](https://zenn.dev/ymmt1089/articles/20220513_next_p5)を書いた際にGenerativeArtについても色々知ることができたのでまとめてみます。  
GenerativeArtという言葉自体ふんわりしているので、GenerativeArt"界隈"のことをざっくりリンク貼り付けてる感じです。  
人物、イベント、サイトの3つの観点でまとめました。  
GenerativeArt"界隈"の情報収集の助けになれば幸いです。  

# GenerativeArtとは
明確な定義はないようですが、個人的に「数学的なアルゴリズムを用いて表現された幾何学的なもの」だと思っています。  
以下のサイトで見れるものが、自分が思うGenerativeArtに近いです。  
http://www.bnn.co.jp/support/generativedesign_p5js/

この記事ではProcessingやp5jsで作成できるもののことをGenerativeArtと呼びます。  

# 目次
* 人物
  * 田所淳
  * takawo shunsuke
  * NIINOMI
* イベント
  * PCD Tokyo
  * つぶやきProcessing
* サイト
  * NEORT
  * artblocks
  * OpenProcessing


# 人物
## 田所淳
https://twitter.com/tadokoro?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor

Processing,p5js,GenerativeArt,openFrameworks,ライブコーディング,etc... この辺りの単語を調べて出てくる書籍の著者に必ずと言っていいほど名前があります。  
田所さんのサイト、[yoppa](https://yoppa.org/)では書籍の紹介のほかにも講義ノートやスライドなどを無料で公開されてます。(とてもありがたい🙏)  
講義資料であるために初心者にもわかりやすく書かれていているため、「興味はあるけどどこから始めよう。。。」という時にとても参考になります。  

## Takawo Shunsuke
https://twitter.com/takawo?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor

デイリーコーディング(つぶやきProcessing)のTwitterのタグで知りました。  
デイリーコーディングの人、継続して作品を生み出す人の印象が強いです。  
[高尾さんのサイト](https://cenkhor.org/)でも講義の資料が公開されています。  

高尾さんはNFTアートプロジェクト、[Generativemasks](https://generativemasks.io/)を発表した人です。  
Generativemasksはプログラムで作成された1万点のNFTアートワークを販売したプロジェクトで、発表から2時間ほどで完売したことで話題になりました。  

こちらのインタビュー記事ではどのような経緯でGenerativemasksを立ち上げる背景について知れます。　　
特にNFTアートを販売する目的ではないことや、高尾さんの収益は寄付されていることなどが読み取れます。  
https://sb-rs.com/article/2548

## NIINOMI
https://twitter.com/r21nomi

[NEORT](https://neort.io/popular)のCEOです。
自身もさまざまな作品を発表されています。
https://niinomi.art/about


# イベント
## PCD Tokyo
https://pcd-tokyo.github.io/

引用
> PCDとは、Processing Community Day(プロセッシング・コミュニティー・デー)の略です。
> Processing のユーザーの交流イベントで、アートとプログラミングのコミュニティの多様性を祝って 世界各地で開催されています。
> 日本では ライゾマティクス 真鍋大度 様、イレブンプレイ MIKIKO 様、OpenProcessing 様の協力のもと、2021年2月20・21日に オンライン にて開催されま> す。
> Processing のパイオニアや若手アーティストらによる講演やワークショップを通じて、
> クリエイティブなハックやアートに直に触れることができます。

2019年から始まったイベントで、Processingを用いた表現で有名な方が参加されています。2019年はオフラインイベントでしたが、コロナの影響で2021年はオフライン開催でした。  
PCD Tokyoで名前が上がる方のtwitterを追うだけでGenerativeArt界隈のトレンドについて知ることができます。

## つぶやきProcessing
`#つぶやきProcessing` はtwitterのハッシュタグです。  
このタグではProcessingを使用したGenerativeArtのツイートを見ることができます。  
またtwitterで表現する以上、1ツイートに収まる文字数制限以内で完成させる必要があるため、記述されているコードはかなり簡略化されています。  
つぶやきProcessing界隈ではコードの文字数を少なくし、いかに表現の幅を広げるかに集約されており、[tips](https://note.com/aq_kani/n/nc25b1f26ba7d)を書かれている人もいます。  
※ツイートの文字制限は280文字ですが、`#つぶやきProcessing` のハッシュタグを含めると260文字。。。

また、つぶやきProcessingに関わる情報をリツイートするアカウントもあります。
https://twitter.com/tweetprocessing

# サイト
## NEORT
NIINOMIさんの作成されたアートプラットフォームです。  
誰でも作品を登録でき、またNFTアートとして販売することもできます。
https://neort.io/latest

## artblocks
こちらもNFTアートとして販売・購入できる海外のサイトです。  
少し変わっているのが作品が生成されるまでの仕組みです。　　
クリエイターがGenerativeArtを生成するスクリプトをアップロードして販売し、購入者が購入した時点でスクリプトが実行、ランダムにデザインが生成されることで初めて作品になります。  

https://www.artblocks.io/

## OpenProcessing
Processingで作成した作品をを公開したり、共有したりできるサイトです。  
気になった作品があれば、コードを見ることができるので、どのようなロジックで表現されているかを知ることができます。  

https://openprocessing.org/

# 最後に
GenerativeArt界隈は公開されている情報が多く、また、アートである以上正解がありません。  
自分がいいと思った表現をNFTアートとして販売できる環境も整っています。  
今回紹介させてもらった人物やイベントやサイトにそれぞれ目的や思想があったように、自分がいいと思うものをGenerativeArtととして表現できればいいなと思います。
