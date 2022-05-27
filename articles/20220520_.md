---
title: "Babylon.jsでキューブを表示してみた"
emoji: "🧊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['Babylon.js','TypeScript','WebGL']
published: false
---

# はじめに
2022年5月6日にbabylon.jsのバージョン5.0.0が公式にリリースされましたね。　
5.0が発表するまでのstable版は4.2.0で2020年11月13日のリリースだったので、待ちに待った待望のリリースだったようです（他人事）  
[やまみさんの記事](https://zenn.dev/yamayuski/articles/c5dfa2c53359c4)参考。

時代はメタバース()ということで、せっかくの機会なので触ってみました。  
四角いキューブを表示してグリグリ動かすまでを目指します。  
初心者視点で動かした所感の記事になります。  

キューブをグリグリ動かす図↓
![](https://storage.googleapis.com/zenn-user-upload/4acc8f662898-20220527.gif)


# そもそもBabylon.jsとは？
Babylon.jsはWebGLのライブラリで、Microsoftが提供しています。  
WebGLのライブラリとしてはthree.jsが有名です。three.jsと比べるとBabylon.jsはあまり使われていないようです。  
https://www.npmtrends.com/babylonjs-vs-three

しかし、three.jsに比べてBabylon.jsには、以下のようなメリットがあります。  
* TypeScriptのサポートがある
* [PlayGround](https://playground.babylonjs.com/)でコードや挙動を簡単に試せる
* 公式ドキュメントが丁寧かつ豊富

特にPlayGroundが使えるのはかなり強力です。PlayGroundを使えば、記述したコードが表示される3Dモデルにどのように影響を与えるかを目視できます。  
WebGLではシェーダーやライトの設定などの組み合わせで表示が変わるため、PlayGroundで動きを確認できることは重要なメリットのように思います。  
またTypeScriptのサポートが手厚いので、TypeScriptを使用したプロジェクトへの導入がしやすいことや、型推論で使用できる関数の補完ができるのでかなり直感的に使用できます。

# 使ってみる
## 手順

## 環境

環境は以下です。  
create-next-appで作成したプロジェクトの1画面でBabylon.jsのキューブを表示させます。

| 項目 | バージョン詳細 |
| ---- | ---- |
| node | 16.14.2 |
| next | 12.1.4 |
| react | 18.0.0 |


## インストール
早速インストールします。

```
yarn add -D  @babylonjs/core
```

Babylon.jsのインストールには `@babylonjs/core` と `babylonjs` があります。  
この2つの違いはTree-Shakingが利用できるかの違いで、@babylonjs/coreでインストールすればTree-Shakingが利用できます。  
基本的に `@babylonjs/core` でインストールでいいかなと思います。  
Webpackを使用するプロジェクトが多いですし、Tree-Shakingが利用できるのであれば使いたいですしね。

## コード記述
初めにコード全文を記載します。  
各コードの説明は後述します。

Nextを使用しているので、pages配下にindex.tsxファイルを作成して以下のコードを記述します。  
（ツッコミどころが多いコードだと思いますが、多めにみてください🙇‍♂️）

```tsx : /pages/babylonjs/index.tsx
import { NextPage } from "next";
import * as BABYLON from "@babylonjs/core";
import { useRef } from "react";

const Babylonjs: NextPage = () => {
  const ref = useRef(null);

  (async () => {
    // engineを作るためにはcanvasが必要
    const renderCanvas = document.getElementById(
      "renderCanvas"
    ) as HTMLCanvasElement | null;

    // sceneを使用するためにはenginが必要
    // 第二引数でアンチエイリアス（モデルのエッジのギザギザを目立たなくするか）
    const engine = new BABYLON.Engine(renderCanvas, true);
    const scene = new BABYLON.Scene(engine);

    // カメラとライトの設定
    scene.createDefaultCameraOrLight(true, true, true);
    // 環境？の設定
    scene.createDefaultEnvironment();

    // モデル（今回はキューブの設定）
    const boxSize = 0.3;
    const box = BABYLON.MeshBuilder.CreateBox("box", { size: boxSize });

    box.position.addInPlaceFromFloats(0, boxSize / 2.0, 0);

    engine.runRenderLoop(() => {
      scene.render();
    });
  })();

  return (
      <canvas
        id="renderCanvas"
        style={{ width: 1440, height: 800 }}
        ref={ref}
      ></canvas>
  );
};

export default Babylonjs;

```

これだけで画面にはキューブが表示されていると思いますので確認しましょう。


## 表示確認

はいできた🧊
![](https://storage.googleapis.com/zenn-user-upload/4f948e94bda4-20220527.gif)

拡大や縮小もできるしヌメヌメ動きます。  
下から覗き込んだ際はキューブに光の反射？光沢のようなものが見えて良いです。  

WebGL系のライブラリでは、本来はシェーダーの設定やライトの設定などが必要で、その分コード数も多くなります。  
しかし今回は少ない行数でキューブの表示ができました。  

## コード解説
コードを簡単に解説していきます。

### 基礎部分設定
```tsx
    // sceneを使用するためにはenginが必要
    // 第二引数でアンチエイリアス（モデルのエッジのギザギザを目立たなくするか）
    const engine = new BABYLON.Engine(renderCanvas, true);
    const scene = new BABYLON.Scene(engine);
```
ここではengineとsceneの設定をしています。  
engineは3Dを表示するための基礎の部分ですね。engineの上に新しくsceneを作成しています。  
ここからは作成したsceneの上にカメラや照明やモデルなどを配置していくことになります。  

### カメラ、照明、環境の設定
```tsx
    // カメラとライトの設定
    scene.createDefaultCameraOrLight(true, true, true);
    // 環境？の設定
    scene.createDefaultEnvironment();
```
`createDefaultCameraOrLight` でカメラと照明の設定を行なっています。  
特に細かく数値を設定する必要はなく、よしなにカメラとライトを設定してくれます。

`createDefaultEnvironment` でどこまでの設定を行なっているかわからないのですが、背景の色など環境部分の設定を行なっていそうです。（詳しくわかれば追記します🙇‍♂️）  

本来であればここでシェーダー、カメラ、照明、etc...の記述が必要で、初めからそれらを記述するのは骨が折れます。　　
なので今回はデフォルトで用意されている関数を利用しました。

ここで使用した2つの関数を利用することで簡単に3Dオブジェクトを表示する環境を構築できます。  
[公式ドキュメント](https://doc.babylonjs.com/divingDeeper/scene/fastBuildWorld#how-to-fast-build-a-world)でも、「このやり方が一番早いと思います（意訳）」って言ってます。

### 3Dモデルの設定
```tsx
    // モデル（今回はキューブの設定）
    const boxSize = 0.3;
    const box = BABYLON.MeshBuilder.CreateBox("box", { size: boxSize });

    box.position.addInPlaceFromFloats(0, boxSize / 2.0, 0);
```
`BABYLON.MeshBuilder.CreateBox` でキューブを作成しています。  
MeshBuilderにはCreateBox以外にもさまざまな形状のモデルを生成する関数が用意されています。  
関数の型情報を見にいけば、引数に何の値を入れれば良いかがわかるので、TypeScript対応の恩恵を感じました。  

最後にaddInPlaceFromFloatsでキューブの表示位置を指定します。

# おわりに
Babylon.jsで簡単にキューブを表示できました。  
three.jsからBabylon.jsnに乗り換えたという記事も目にしたので、これからWebGLのライブラリを使うならBabylon.jsも候補に入れていい気がします。

メタバースだとかxRという単語をよく耳にするようになりました。  
その流れでWebGLの需要が高まってWebで面白い表現ができるようになると楽しそうですね。


# 参考記事
https://doc.babylonjs.com/
https://zenn.dev/drumath2237
https://techbookfest.org/product/6028242932727808?productVariantID=5465292979306496
https://zenn.dev/yamayuski/articles/fdcbf2c0a4bed0
https://qiita.com/soarflat/items/755bbbcd6eb81bd128c4
https://qiita.com/il-m-yamagishi/items/fa0460830de7a17a8f89
