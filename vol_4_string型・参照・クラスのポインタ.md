#C++講座資料

##string型

C++標準ライブラリのstring型を使うと、Cのchar型に比べ、より簡単に文字列を扱うことが出来る。
+演算子で文字列の結合、==演算子で文字列の比較が出来る。  
string型を使用するときは、stringヘッダーをインクルードする。（string.hではないことに注意）

```cpp
#include <iostream>
#include <string>

int main(){
    std::string str1 = "aaa";
    std::string str2 = "bbb";
    
    std::cout << str1 << std::endl;
    std::cout << str2 << std::endl;
    
    std::string str3;
    // +で文字列の結合ができる
    str3 = str1 + str2;
    
    std::cout << str3 << std::endl;
    
    // ==で文字列の比較ができる trueなので1が出力される
    std::cout << (str3 == (str1+str2)) << std::endl;
    
    return 0;
}
```


>std::to_string関数を使うと、int型、double型等の変数をstring型に変換できる。

```cpp
#include <iostream>
#include <string>

int main(){
    std::string str = "aaa";
	int ix = 23;
	double dx = 3.14;
	
	str += " ";
	str += std::to_string(ix);
	str += " ";
	str += std::to_string(dx);
    
    std::cout << str << std::endl;
    
    return 0;
}
```

##bool型
C言語では、0は偽、それ以外は真だったが、C++には真偽を表すためだけの型、bool型が存在する。  


```cpp
#include <iostream>

int main(void){

	bool b = true;

	if (b){
		std::cout << "条件文は真" << std::endl;
	}
	else{
		std::cout << "条件文は偽" << std::endl;
	}

	return 0;
}
```

以下のように、条件を満たすかどうかの真・偽を返す関数の返り値の型としてbool型を使うことが多い。

```cpp
#include <iostream>

bool IsEven(int num){
	return (num % 2) == 0;
}

int main(void){

	if (IsEven(10)){
		std::cout << "条件文は真" << std::endl;
	}
	else{
		std::cout << "条件文は偽" << std::endl;
	}

	return 0;
}	
```


##スコープ

{ } で囲まれた部分を抜けると、その{}内部で宣言された変数は破棄されるので、アクセスできなくなる。
変数や関数の「見える」範囲をスコープと言い、変数の寿命はスコープによって決まる。


```cpp
#include <iostream>

void Func(){
	int a = 10;
	std::cout << "aの値は " << a << std::endl;
}

int main(){
	
	Func();

	//コメントを外すとコンパイルエラー
	//a = 100;  

	if(true){
		int b = 20;
		std::cout << "bの値は " << b << std::endl;
	}
	
	//コメントを外すとコンパイルエラー
	//b = 100; 

	for(int i = 0; i < 3; i++){
		std::cout << "iの値は " << i << std::endl;
	}
	
	//コメントを外すとコンパイルエラー
	//i = 100; 

	return 0;
}
```


>{}だけでもスコープを作ることができる

```cpp
#include <iostream>

int main(){
	{
		int b = 20;
		std::cout << "bの値は " << b << std::endl;
	}
	
	//コメントを外すとコンパイルエラー
	//b = 100; 

	return 0;
}
```

##参照

swap関数を実装するためにポインタを用いる事があったかもしれない。 C++ではそれを扱いやすくした参照(リファレンス)というものが存在する。  


>ポインタを用いたaとbを入れ替える関数

```cpp
#include <iostream>
//aとbを入れ替える関数 
void swap(int *a,int *b){
    int tmp=*a;
    *a=*b;
    *b=tmp;
    return;
}

int main(){
 
    int a=3;
    int b=5;
 
    std::cout << a << "," << b << std::endl;
    
    swap(&a, &b);//aとbのポインタを渡さなければいけない
    
    std::cout << a << "," << b << std::endl;
 
    return 0;
}  
```

>よくない例。下のように書くと、関数内でaとbのコピーが出来て、コピーのaとbが入れ替えられ、関数を抜けた時点で破棄されるので、main関数内のaとbは入れ替えられない。

```cpp
#include <iostream>
//aとbを入れ替える関数 値渡しなので入れ替えられない
void swap(int a,int b){
    int tmp=a;
    a=b;
    b=tmp;
    return;
}

int main(){

    int a=3;
    int b=5;
 
    std::cout << a << "," << b << std::endl;
    
    swap(a, b);
    
    std::cout << a << "," << b << std::endl;
    
    return 0;
}
```

>参照を用いたaとbを入れ替える関数

```cpp
#include <iostream>
//aとbを入れ替える関数
void swap(int& a,int& b){
    int tmp=a;
    a=b;
    b=tmp;
    return;
}

int main(){
 
    int a=3;
    int b=5;
 
    std::cout << a << "," << b << std::endl;
    
    swap(a, b);
    
    std::cout << a << "," << b << std::endl;
 
    return 0;
}
```

>参照を用いてHogeに別名をつけることが出来る。

```cpp
#include <iostream>

int main(){
 
    int Hoge = 2;
 
    //aliasはHogeの別名(aliasを操作するとHogeの値が変わる)
    int& alias = Hoge;
    
    std::cout << Hoge << ", " << alias << std::endl;
    
    alias=4;
    std::cout << Hoge << ", " << alias << std::endl;
 
    return 0;
}
```

>ポインタを使っても同じことが出来るが、*や&をつけるのに手間がかかり、ミスをしてバグを起こしやすいので、これからは参照を使おう。

```cpp
#include <iostream>

int main(){
 
    int Hoge = 2;
 
    int* ptr = &Hoge;
    
    std::cout << Hoge << ", " << *ptr << std::endl;
    
    *ptr = 4;
    std::cout << Hoge << ", " << *ptr << std::endl;
 
    return 0;
}
```

## const参照渡し
関数がクラスを引数に取る時、オブジェクトのコピーが発生している。参照渡しを使うと、コピーが発生することを防ぐことが出来、無駄がなくなる。
また、引数にconstを付けると、関数内で値を変更できなくなる。これは間違って値を書き換えてしまうのを防ぐのに有用である。
以下は、const参照渡しを使う関数の例である。本資料では以降クラスを引数に取る時、適宜参照渡し、const参照渡しを使っていく。

```cpp
#include <iostream>

class Color {
public:
	int r, g, b;
	Color(int r_, int g_, int b_) {
		r = r_;
		g = g_;
		b = b_;
	}
};

void ShowColor(const Color& color) {
	std::cout << "r:" << color.r << " g:" << color.g << " b:" << color.b << std::endl;
	// コメントを外すと値を書き換えることになるのでコンパイルエラー
	// color.r = 0;
}

int main() {

	Color color(150, 100, 50);

	ShowColor(color);

	return 0;
}
```


##演習問題

1. 文字列をうけとりその末尾に「その点トッポってすげぇよな、最後までチョコたっぷりだもん。」と付加するような関数を作れ。  

1. 空の文字列を用意し、入力された文字をその文字列の終端に付け加えていき、"Show"と入力されたら現在の文字列を表示せよ。


##ポインタ（復習）

>2通りの方法でaの値とアドレスを表示

```cpp
#include <iostream>

int main(void){

	int a = 100;
	int *ptr = &a;

	//2通りの方法でaの値とアドレスを表示
	std::cout << "aの値:" << a << "  aのアドレス:" << &a << std::endl;
	std::cout << "aの値:" <<  *ptr << "  aのアドレス:" << ptr << std::endl << std::endl;

	return 0;
}
```

##オブジェクトへのポインタ

オブジェクトを指すポインタからオブジェクトの要素にアクセスしたい時がある。  
下の例では、オブジェクトを指すポインタ、ptrから、objのxにアクセスしたい。  
(\*ptr).xと書けばobjのxにアクセスできるが、少々書きづらいので、  
ptr->xと書くことができるようになっている。  
(\*ptr).x と ptr->x は同じ意味である。  
-> をアロー演算子という。  

ちなみに、\*ptr.xと書くと\*(ptr.x)と解釈され、ptrのx（存在しない）が指すものを表す。

```cpp
#include <iostream>

class Vector2{
public:
	int x, y;

	Vector2(int xx, int yy)
	{
		x = xx;
		y = yy;
	}
};

int main(void){

	Vector2 obj(12, 34);
	Vector2 *ptr = &obj;
	
	//普通に表示
	std::cout << obj.x << " " << obj.y << std::endl;

	std::cout << (*ptr).x << " " << (*ptr).y << std::endl;

	std::cout << ptr->x << " " << ptr->y << std::endl;


	//↓このようには書けない。　*(ptr.x)と解釈されるから
	//std::cout << *ptr.x << " " << *ptr.y << std::endl;

	
	return 0;
}
```



##演習問題(DXライブラリ)
今回のは、これまでコードを書いてきたプロジェクトとは別に、サンプルプロジェクトからプロジェクトを作ることを推奨します。

1. string型の変数を用意し、DrawFormatStringToHandle関数を用いて画面中央に表示せよ。はじめの文字列は入力を求めるのではなく、プログラム内で決めてよい。

* DrawFormatStringToHandle関数は引数にstring型ではなくchar\*を取る。  (string型変数).c_str()関数を使うことでstring型の文字列をchar\*へ変換できる。

	>例

	```cpp
	string str = "aaa";
	DrawFormatStringToHandle( x , y , Color , FontHandle , str.c_str() ) ;
	```


1. Xキーを押した時に、上記の文字列の末尾に「かわいい」が追加されるようにせよ。

1. 以下の様なPlayerクラスとEnemyクラスを用意した。PlayerクラスとEnemyクラスのインスタンスを作り、動作を確認せよ。

>Player.h

```cpp
#pragma once

class Player {
public:
	double x, y, v;
	Player();
	void Update();
	void Draw();
};
```

>Player.cpp

```cpp
#include <DxLib.h>
#include "myglobal.h"
#include "Player.h"


Player::Player() {
	x = 400;
	y = 400;
	v = 4.5;
}

void Player::Update() {
	// 上下左右キーで移動
	if (keyState[KEY_INPUT_RIGHT] > 0) {
		x += v;
	}
	if (keyState[KEY_INPUT_LEFT] > 0) {
		x -= v;
	}
	if (keyState[KEY_INPUT_UP] > 0) {
		y -= v;
	}
	if (keyState[KEY_INPUT_DOWN] > 0) {
		y += v;
	}
}

void Player::Draw() {
	DrawCircle(x, y, 24, GetColor(0, 0, 255), 1);
}

```

>Enemy.h

```cpp
#pragma once
#include "Player.h"

class Enemy {
public:
	double x, y, vx, vy;
	Player *player;
	Enemy(double xx, double yy);
	void Update();
	void Draw();
};

```

>Enemy.cpp

```cpp
#include <DxLib.h>
#include "Enemy.h"

Enemy::Enemy(double xx, double yy) {
	x = xx;
	y = yy;
	vx = 0;
	vy = 1.0;
}

void Enemy::Update() {
	x += vx;
	y += vy;
}

void Enemy::Draw() {
	DrawCircle(x, y, 24, GetColor(255, 0, 0), 1);
}

```


1. EnemyがPlayerの方向に移動するようにしたい。Enemyクラスが「Playerクラスへのポインタ」をメンバに持つようにして、PlayerクラスとEnemyクラスのインスタンスを生成した後にEnemyのインスタンスににPlayerのインスタンスのポインタを渡し、そのポインタからPlayerクラスのx,yにアクセスすることでEnemyがPlayerの位置を取得し、その方向に移動できるようにせよ。  
（今回は、敵の追尾は大まかで良い。例：PlayerのxがEnemyのxより大きければEnemyは右に、そうでなければ左に動く…など）

>ヒント

プログラムは、上から順にコンパイラに解釈されていく。Enemyクラス内でPlayerクラスにアクセスするには、アクセスする前にPlayerクラスの中身をコンパイラが知らなければならない。  
今回の場合、main.cppの上部で、 \#include "Player.h" が \#include "Enemy.h" より前に書かれていれば、コンパイラがPlayerの中身を知ってからEnemy.hを解釈するので問題ない。  
\#include "Enemy.h" を \#include "Player.h" より前に書きたいときは、Enemy.hがインクルードされる前にどこかのヘッダー、またはEnemy.hの上部に \#include "Player.h" があればよい。  
Enemy.hとmain.cppの二箇所からPlayer.hを読み込むときは、多重インクルードを防ぐため、Player.hの上部に \#pragma once をつける必要がある。  


>Enemy側

```cpp
class Enemy{
public:
	double x, y, vx, vy;

	Player* pPlayer;//これを追加

	//ポインタ取得用関数
	void SetPlayerPtr(Player* ptr){
		pPlayer = ptr;
	}

	//以下略
};
```

>初期化処理側

```cpp
Player player;//Playerのインスタンスを生成
Enemy emy;//Enemyのインスタンスを生成
emy.SetPlayerPtr(&player);//ポインタを入れる
```
