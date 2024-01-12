---
title: "幽霊文字について調べてみた"
emoji: "👻"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["幽霊文字", "unicode", "文字コード"]
published: true
---

# 幽霊文字とは

幽霊文字とは、辞書に掲載されていないにも関わらず、文字コード上に存在する漢字のことを指します。これらの文字は存在しないはずのものなのに、なぜか文字コードに含まれています。辞書に登録されているが実用例のない単語を意味する「幽霊語」をもじって「幽霊文字」と呼ばれているようです。代表的なものとして「妛」や「彁」が挙げられます。

↓ こういうやつです。  
なんかみたことあるような、読めそうで読めないようなやつです。  
![](https://storage.googleapis.com/zenn-user-upload/69c796d10a5c-20240104.png)

![](https://storage.googleapis.com/zenn-user-upload/ee42a883eaec-20240108.png)

![](https://storage.googleapis.com/zenn-user-upload/608373154566-20240108.png)

# なぜ幽霊文字が存在するか

幽霊文字が存在する主な理由は、文字コードの作成過程でのヒューマンエラーです。文字データベースの作成時に、誤って非実在の文字が含まれてしまったり、誤植や誤解が原因で実在しない文字が採用されることがあります。これらが後に「幽霊文字」として知られるようになりました。

例えば、「妛」の文字は「𡚴」の誤植です。「𡚴」を文字コードに起こす前、紙に「山」と「女」の漢字を縦に合体させて表示していました。これを貼り合わせた際に、「山」と「女」の間に影のような棒線が出てしまい、見間違えで間に「一」が混じった「妛」という文字として登録されたことによるものらしいです。

参考: https://sp.volkswagen.co.jp/LINE/page1/

# 幽霊文字が修正がされない理由

どの文字コードが幽霊文字かはある程度把握されています。把握できているなら次のJIS改正で修正すれば良いのでは？と、思いますがそうできない理由がいくつかあります。

## 互換性がないことや既に Unicode に登録されている問題

かつて [83JIS](https://the-simple.jp/what-is-83jis-an-easy-to-understand-explanation-of-the-basic-concepts-of-computer-terminology) 改正で非互換の改変がされたために大きな混乱を招いたことがあり、同じ混乱を起こさないためにも文字コードの改正は慎重に行う必要が出てきました。  
また、幽霊文字が判明した時点で既に、 Unicodeにて幽霊文字を含んだJIS基本漢字が収録されておりJIS規格の範疇で修正できるものではなくなってしまいました。（というより修正後の影響範囲が広すぎて幽霊文字を置き換えることはほぼ不可能になっているのかなと思います）

一旦Unicodeについての問題は置いておいたとしても、この手の話で難しい原因は既存のシステムやデータベースとの互換性です。一度文字コードに含まれると、そのコードを使用しているシステムやデータが多数存在するため、単純に削除することが困難になります。また、これらの文字が歴史的な文書や古文書に使われている可能性もあり、その意味でも保存する価値があるとされています。  
過去に起きた文字コードの改正によって起きた混乱の1つが森鴎外問題です。

## かつての森鴎外問題を繰り返したくない

日本の文字コードは、その歴史の中で幾度もの改正が行われてきました。これらの改正は、技術的な発展と共に必要とされたものですが、同時に多くの混乱をもたらしたようです。  
調べた中で特に分かりやすい事例が1983年の [JIS X 0208](https://ja.wikipedia.org/wiki/JIS_X_0208) の改正によって生じた「[森鴎外問題](https://www.pahoo.org/e-soul/webtech/encode/encode10-01.shtm)」です。  
この改正で、多くの漢字字体が変更され、その中には「鷗」という漢字も含まれていました。この字は日本の文豪「森鴎外」の名前に使われていた漢字です。  
83JISの改正では、「鷗」の字体が「鴎」に変更されました。このため、森鴎外の名前をコンピュータで表示しようとすると、改正前の「鷗」ではなく、改正後の「鴎」が使用されるようになりました。  
歴史や純文学に置いて漢字の表現や字体が持つ意味が重要視されることは想像に難くありません。ましてや文豪の名前に使われている漢字が文字コードの改正によって変わるというのは、大きな問題だったことは想像に難くありません。  
この字体の変更は、文学作品や歴史的な文書における正確な表記の重要性を考えると、深刻な問題です。特に、JIS X 0208のような広範囲に使用される文字コードの改正は、予期せぬ混乱を引き起こす可能性があります。

参考: https://www.pahoo.org/e-soul/webtech/encode/encode10-01.shtm

# 結局幽霊文字は今後も残り続ける

結局上記の理由で幽霊文字は修正されずに残り続けています。  
Unicodeも絡んでいることを考えると、日本（JIS規格）だけの問題ではないため今後も修正されることはほぼないのかなと思います。

# 所感

規格の制定の過程で生じた小さなミスが、時間を経て負債として残り続け修正が難しい状況になるのは幽霊文字に関わらずあるあるなのかなと思います。実際に、プロジェクトを運用する上で定めたルールや実装が後の開発において障害となることもある（というより避けては通れない）ことだと思うので、どのタイミングで修正できるのかを見極めたい気持ちを胸に刻む良い機会でした。  
技術的負債の話になった時に小ネタで幽霊文字の話できるくらいの小ネタでした。

# 参考

https://youtu.be/SC48k-KIT-U?si=z6AaRK43bpGDQw24  
https://ja.wikipedia.org/wiki/%E5%B9%BD%E9%9C%8A%E6%96%87%E5%AD%97  
https://www.ogis-ri.co.jp/otc/hiroba/technical/program_standards/part1.html  
https://www.asahi-net.or.jp/~ax2s-kmtn/ref/jis78-83.html  
https://www.pahoo.org/e-soul/webtech/encode/encode10-01.shtm
http://shimax.cocolog-nifty.com/search/2006/11/ie7_ee15.html