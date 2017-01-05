#役立ちそうなサイト・キーワード紹介

C++の便利な機能(STL)、役立ちそうなサイト、調べると良いキーワード・概念などを列挙してみた。一応ゲームに役立ちそうな順に並べている。以下の機能は何となく把握しておいて、ゲーム開発中に使えそうな機能を思い出したら詳細に調べるようにするといいと思う。  

## cpprefjp  
https://sites.google.com/site/cpprefjp/  
各種ヘッダーの解説がある。わからないことがあったらここで調べよう。   
暇な時に読んで、役立ちそうなものがないか調べておくといい。   

## ロベールのＣ＋＋教室
http://www7b.biglobe.ne.jp/~robe/cpphtml/  
C++全般について解説。わかりやすい。また、書籍化もされているがそちらもオススメ。
  
## VisualStudioの機能  
VisualStudioには多彩なショートカットやデバック機能などがある。機能を使いこなすことで開発時の効率を向上することができるので（特にブレークポイントは重要）、一度目を通すことを強くオススメする。
  
初級者向けVisualStudio便利機能集Vol.1  
http://qiita.com/hart_edsf/items/4d1561751a96d145301a

## std::map  
キーと値を対応づけられる。とても便利。  
詳しくは上記cpprefjpとかを見るといい。  
mapに比べ少し機能が制限されているが要素の検索速度が早い、unordered_mapもあるので調べてみよう。  

## 名前空間  
大規模なプロジェクトを作るときに必要。  
ロベールのＣ＋＋教室 - 第５０章 異姓同名 -  
http://www7b.biglobe.ne.jp/~robe/cpphtml/html02/cpp02050.html

## 演算子オーバーロード
自作のクラスに+, -などを実装できる。  
例：自作Vector2クラスでこういうことができる

```cpp
Vector2 pt1, pt2, pt3;
pt3 = pt1 + pt2;
```

## デフォルト引数  
ロベールのＣ＋＋教室 - 第６５章 ナマケモノ -  
http://www7b.biglobe.ne.jp/~robe/cpphtml/html01/cpp01065.html

## テンプレート  
テンプレートメタプログラミングは深淵だが、通常のテンプレートは使えたら便利。  

## range-Based-for  
以下のように、コンテナの各要素に対しての処理を簡易に書ける。  

```
for(const auto& element: vec){
        std::cout << element << std::endl;
    }
```

## ラムダ式  
外部変数のキャプチャなど、講座でやったことに比べてもう少し色々できるので調べてみよう。      
  
C++11メモ @ ラムダ式で無名関数やクロージャ  
http://ramemiso.hateblo.jp/entry/2013/12/22/225039
  
## functionalヘッダーとラムダ式  
std::function　：　通常の関数に加え、ラムダ式を受け渡しできる。    
関数を変数に入れておける。（関数ポインタの汎用性高いバージョンみたいなもの）  
ボタンクラスを用意して、ボタン押されたら実行する関数をラムダ式でボタンのコンストラクタに渡す、などが出来る  

## randomヘッダー
randより性能の良い（よくバラけている）乱数を生成できる。正規分布なども可能。

## std::weak_ptr  
指している対象が消えていたらfalseを返す関数とかを持ってる。
シューティングを作るときにホーミング弾などの実装に便利。  

## tuple, pair  
tupleとpairは複数のクラスの組をまとめて持てる。単純だが、地味に使うと思う。  

## deque  
deque(デック)はvectorの親戚みたいなやつ。  
詳しくはcpprefjp参照。  

## 翻訳単位  
インクルードなどの理解を深めたいならこの単語でググるといい。    
      
## デザインパターン
C++の機能ではなく、プログラムのうまい組み方、みたいなもの。  
シングルトンもこれの一種。  

デザインパターン | TECHSCORE(テックスコア)  
http://www.techscore.com/tech/DesignPattern/index.html  
このサイトはJavaで書かれているが、とてもざっくりした説明なのでおすすめ。

## boost
C++の神々が作っているライブラリ（便利なクラスとか関数がたくさんあるやつ）。  
当たり判定や構文解析（ファイル読み込みに使える）があるので、ゲーム作りに役立ちそう。  

## 参考  
* ラムダ式  
http://cpplover.blogspot.jp/2009/11/lambda.html  
http://d.hatena.ne.jp/gintenlabo/20130516/1368711542  
http://d.hatena.ne.jp/gintenlabo/20130526/1369565989  
  
* randomヘッダーなど  
http://www.slideshare.net/Reputeless/c11c14  
  
* オブジェクト指向  
http://www.slideshare.net/MoriharuOhzu/ss-14083300  
  
* csvを読み込む  
http://d.hatena.ne.jp/eiki_okuma/20110816/1313507114  
    
* 計算幾何(当たり判定)  
https://sites.google.com/site/boostjp/tips/geometry

* シーン遷移  
http://marupeke296.com/IKDADV_ETC_StateTransition.html  
http://marupeke296.com/GDEV_No7_StateImpliment2.html  
http://marupeke296.com/GDEV_No6_SceneClass.html