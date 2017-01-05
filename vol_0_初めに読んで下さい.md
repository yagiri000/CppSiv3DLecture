#C++講座資料   

## C++講座資料 vol_0  
本資料は、某大学サークル(CCS)の講座向けにやぎりが作成した、C++初学者向け資料です。DXライブラリとC++で実際にプログラムを組んで、「C++の機能(主にクラス,STLのvector,継承・ポリモーフィズム)をゲームを作るにあたって実際にどう使うか何となく理解」することが目的になっています。C++の本を読み、配列や関数は何となく使えるけどゲームを作るにあたってクラスをどう使えばいいのかわからない、クラスというものがそもそもよくわからないといった人向けです。  
資料全体の流れとして、実際にSTGもどきを作っていきます。  
また、開発環境はWindows, Visual Studio 2015、DxLibのバージョンは3.16dを想定しています。  
今回は[添付資料](ああああ)内の、DxLibサンプルプロジェクトの解説をします。DxLib初心者の方は、[DxLib導入資料講座](http://qiita.com/yagiri000/items/597456ba8ba8435f1e01)も合わせてどうぞ。  
本資料の一部は、[いかろちゃんのサークル用C++講座資料](https://turarasoft.com/main/ccs/index.html)を参考に作られました。ありがとうございました。  

## 本資料のやり方
基本的にコードをコピペして自分で動かして確認、少し書き換えて思った通りになるか確認しながら進んでください。たまにC++の闇が顕現することがありますが、その場合は次の項目に進んで、後で戻ってきて考えるといいと思います。また、概要だけ説明するため、いろいろな話が割愛されているので、詳細が気になる場合はとりあえず無視して進むか、気になる場合はネットでC++の各機能を調べながら進むといいです。  
あと、全体的に難しい（※人による）ので、途中で心が折れても気にしないほうがいいです。気が向いたら戻ってきてください。  

## 演習問題について
<DxLib.h>をインクルードしているプログラムはDxLibサンプルプロジェクトを書き換えて解くことを、そうでないプログラムはコンソールアプリケーション(文字だけ出てくる黒画面のやつ)で解くことを想定しています。  
*演習問題の解答は、基本的にはVol1~Vol7までのものはVol8に、Vol9~Vol12のものはVol13に書いてあります。*また、Vol7,10のDxLibの解答例、前半課題と最終課題の解答例は添付資料の中にあります。

## 添付資料内のファイルについて
* DxLibサンプルプロジェクト
ひな形になるプロジェクトです。DxLib演習問題はこれを書き換えて解いていってください。DxLibのフォルダの位置が違う場合は、適宜修正して使ってください。  

* 解答例コード
Vol7,Vol10,前半課題,最終課題の解答例です。（ファイル数が多いのでフォルダごと添付する形にしました）  

* サンプルゲーム.exe
前半課題、最終課題ではこれに似たゲームを作ってもらいます。


## 各回の資料について
1. クラスの基本  
クラスの基本的な書き方を学ぶ。DXライブラリを用いた演習では、上下左右に移動するプレイヤーを作成する。  

1. ファイル分け
クラスのファイル分けの方法を学ぶ。「クラスの基本」で作成したDxLibのプロジェクトのプログラムを実際分割する。  

1. vectorの基本
要素数を変更可能な動的配列"vector"を用いて、敵を複数生成・表示する。  

1. string型・参照・クラスのポインタ
char型より便利な文字列を扱う型であるstring型について学ぶ。また、オブジェクトから他のオブジェクトの情報を得るための参照・ポインタについて学び、DXライブラリの実例ではEnemyからPlayerの情報を参照する。以降の章で学ぶイテレータはポインタを模して作られているので、そのための予備知識としての位置づけでもある。

1. 関数オーバーロード・コンストラクタ・デストラクタ
また、クラスを生成したときと破棄したときに呼ばれる、コンストラクタとデストラクタについて学ぶ。

1. イテレータ・vector要素の削除
vector要素を削除するやり方を学ぶ。敵を削除できるようになる。vectorの要素の削除方法は後述のremove_if,ラムダ式を用いた方法もある。（そちらの方法だと削除時に処理を行いづらく、new,deleteの具体例で若干困るのでこちらも紹介した）

1. コンポジション・ファイル分け
クラスの中にクラスを持つコンポジション、実際にシューティングもどきを作るためのファイル分け、相互インクルードを学ぶ。また、ゲーム中全てのオブジェクトを要素に持つManagerクラスを作る。

1. 前半課題&演習問題の解答
これまで習得した知識でサンプルゲーム.exeと同じシューティングもどきを作成する。正直インクルードなどややこしく、引っかかると詰む可能性があるので、詰んだら敵だけなど作れる部分だけ作った上で、解答を見て飛ばしてもいいかもしれない。

1. new・delete・基底・派生クラス
動的にメモリを確保・開放する機能new, deleteについて学ぶ。また、オブジェクト指向に関してある基底・派生クラス

1. 仮想関数・ポリモーフィズム
*本編*EnemyA,EnemyB,EnemyCを継承を用いて書き、まとめて扱う。

1. スマートポインタ
使わなくなったら自動的にメモリを開放してくれるスマートポインタについて学ぶ。

1. remove_if・ラムダ式
remove_if・ラムダ式を用いてvectorの要素を削除する。  

1. 最終課題&演習問題の解答
前半課題を後半やった内容で書き直し、ポリモーフィズムの有用性を確認する。


## DXライブラリサンプルプロジェクト解説
下の例では、キーの状態を記録する配列とそれを更新する関数を用意して、フォントを指定して文字を描画（DrawFormatStringToHandle）している。  
また、変数にexternをつけて宣言することで複数のcppやヘッダーからその変数にアクセスできるようになる。
（DxLibサンプルプロジェクト→(Dropbox)ああああ　以後、このプロジェクトを書き換えながらDxLibの演習問題を解いていってください。

>myglobal.h

```cpp
#pragma once
#include "DxLib.h"

extern int FontHandle;
extern int mousex , mousey;//マウス座標

//キー取得マウス付き
extern char buf[256];
extern int keyState[256];

//キー入力状態をを更新する関数
void KeyUpdate();
```


>myglobal.cpp

```cpp
#pragma once

#include "myglobal.h"

int FontHandle;//フォント読み込み用変数
int mousex=0 , mousey=0;//マウス座標

//キー取得用の配列
char buf[256] = {0};
int keyState[256] = {0};

//キー入力状態を更新する関数
void KeyUpdate()
{
	GetHitkeyStateAll(buf);
	for(int i = 0; i< 256; i++)
	{
		if(buf[i] == 1) keyState[i]++;
		else keyState[i] = 0;
	}
}
```


>main.cpp

```cpp
#include <DxLib.h>
#include <iostream>
#include "myglobal.h"


int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了
	

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	//
	//ここで敵やプレイヤーのオブジェクトの実体を作る
	//

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMouseVector2( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新
		
		//説明用・文字を描画：keyState配列（自作）には、各キーが何フレーム押され続けているかが入っている
		DrawFormatStringToHandle(300, 300, 0xFFFFFF, FontHandle , "Z KEY %d", keyState[KEY_INPUT_Z]) ;

		//
		//　ここに敵やプレイヤーのUpdate,Drawを書く
		//

		//左上に文字（マウスの座標）を描画
		DrawFormatStringToHandle(20, 20, 0xFFFFFF, FontHandle , "MX:%3d MY:%3d", mousex,mousey) ;

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```

## キーが何フレーム押されているかを取得
GetHitkeyStateAll関数は引数にとった配列に各キーの状態（押されていれば1, そうでなければ0)を入れる。  
下のような関数を自分で作れば、どのキーが何フレーム押されているかを配列に入れることができる。(この関数は毎フレーム呼ばなければいけない)

```cpp
//キー取得用の配列
char buf[256] = {0};
int keyState[256] = {0};

//キー入力状態を更新する関数
void KeyUpdate()
{
	GetHitkeyStateAll(buf);
	for(int i = 0; i< 256; i++)
	{
		if(buf[i] == 1) keyState[i]++;
		else keyState[i] = 0;
	}
}
```

## フォントを指定して文字を描画
DrawFormatStringToHandle関数を使えば、座標、色、フォントを指定して文字を描画できる。printfと同じように%dなどが使える。

```cpp
DrawFormatStringToHandle(20, 20, GetColor(255, 255, 255), FontHandle , "MX:%3d MY:%3d", mousex,mousey) ;
```

## 演習問題
1. double型の変数x, yを用意し、DrawCircle関数を用いてx,yに半径20pxの赤い円を描画せよ。

1. 矢印キーを押したとき上記で描画した円が動くようにせよ。（x、yに数字を足せばよい）

1. Zキーを押したフレームにx座標にGetRand(100)を加算し、円が荒ぶる姿を観測せよ。  
ヒント：GetRandはDXライブラリについている乱数を返す関数。GetRand(100)で0~100の数のうちどれかが返ってくる。（0~99でないことに注意！）