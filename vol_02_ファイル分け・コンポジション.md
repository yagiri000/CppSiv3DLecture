#C++Siv3D講座資料

実際にゲームを作る時、ファイル分けをすることが必要になる。  
今回はクラスの内容を.hと.cppに分ける。

##ファイル分け
以下のように、.hと.cppにクラスの定義を分けて書くことが出来る。今後は基本的に.hと.cppにクラスの定義を分けて書くこと。（ただし、解答例ではファイル数が多くなるのを避けるためすべてMain.cppに書いている場合がある）  
.cppには、名前空間を指定するため、クラス名::関数名と書く必要がある。以下は、コンソールでのプレイヤー例。  
>Player.h

```cpp
#include <iostream>

class Player{
public:	
	int x, y;
	Player(int _x, int _y);
	void update();
	void draw();
	void showXY();
};
```

>Player.cpp

```cpp
#include "Player.h"

Player::Player(int _x, int _y) {
	x = _x;
	y = _y;
}

void Player::update(){
	std::cout << "Playerのupdateが呼ばれました。" << std::endl;
}

void Player::draw(){
	std::cout << "Playerのdrawが呼ばれました。" << std::endl;
}

void Player::showXY(){
	std::cout << "PlayerのshowXYが呼ばれました。 x:" << x << " y:" << y << std::endl;
}
```

>main.cpp

```cpp
#include <iostream>
#include "Player.h"

int main(){

	Player player(20, 30);
	player.update();
	player.draw();
	player.showXY();

	return 0;
}
```


## Tips : ショートカット
複数のファイルに分けると関数やクラスの定義を見に行くのに時間がかかるが、VisualStudio2015では、関数、クラスにカーソルを合わせた状態で```F12```を押すと定義に移動できる。もとのカーソル位置に戻りたい場合は、```Ctrl + -```でもとの位置に戻ることができる。```Ctrl + ,```でクラス、関数を検索して移動できる。他にも様々なショートカットがあるので、インターネットで調べるか、コード上で右クリックして出るダイアログを見て調べてみよう。  


##コンストラクタの書き方 その2(メンバイニシャライザ)
コンストラクタの数値の初期化は以下のようにも書くことができる。自分で定義したクラスをなど初期化する場合、こう書いたほうが、インスタンスのコピーが発生しなくなるため処理が早くなる。以後はこの書き方、メンバイニシャライザを使って初期化を行う。  

```cpp
#include <iostream>

class Vector2{
public:
	int x;
	int y;
	
	Vector2(int _x, int _y):
	x(_x),
	y(_y)	
	{
	}
};

int main(){

    Vector2 obj(1, 2);
    std::cout << obj.x << " " << obj.y << std::endl;
    
    return 0;
}
```

##コンポジション
クラスをクラスのメンバにすることが出来る。  
以下の例では、Vector2クラスをまず定義し、PlayerクラスでVector2クラスのメンバを持っている。  

```cpp
#include <iostream>

class Vector2{
public:
	int x, y;
	Vector2(int _x, int _y) :
		x(_x),
		y(_y)
	{
	}
};

class Player{
public:
	Vector2 pos;
	int hp;

	Player(int _x, int _y, int _hp) :
		pos(_x, _y),
		hp(_hp)
	{
	}
};


int main(){
	Player player(100, 200, 64);

	std::cout << "x:" << player.pos.x << ", y:" << player.pos.y << std::endl;
	std::cout << "hp:" << player.hp << std::endl;

	return 0;
}

```

コンポジションをしている場合、コンストラクタは以下のように書く。引数を受け渡すと考えれば良い。  
classAがclassBを持つ場合  

```cpp
classA(引数1,引数2,引数3...) :
	classB(引数1,引数2,引数3...)
	{
	}
```

クラスをコンポジションした場合、クラスに含まれているクラスのコンストラクタが先に呼ばれる。デストラクタは逆の順で呼ばれる。  
HogeやPiyoは日本では、「特に意味のない名前」を表す。  

```cpp
#include <iostream>

class Hoge{
public:
	int x;

	Hoge(int _x):
		x(_x)
	{
		std::cout << "Hogeのコンストラクタが呼ばれました" << std::endl;
	}

	~Hoge(){
		std::cout << "Hogeのデストラクタが呼ばれました" << std::endl;
	}
};

class Piyo{
public:
	Hoge Hoge_;
	int y;

	Piyo(int _x, int _y):
		Hoge_(_x),
		y(_y)
	{
		std::cout << "Piyoのコンストラクタが呼ばれました" << std::endl;
	}

	~Piyo(){
		std::cout << "Piyoのデストラクタが呼ばれました" << std::endl;
	}
};


int main(){

	Piyo piyo(100, 200);

	std::cout << piyo.Hoge_.x << " " << piyo.y << std::endl;

	return 0;
}
```


##インクルード
\#includeを使うと自分が定義したヘッダーをインクルードできる。インクルードすると、その部分にヘッダーの中身のコードが展開される。

##\#pragma once
\#pragma onceを使うと一度読み込まれたヘッダーは読み込まれないようになる。複数のファイルから読み込まれるヘッダーの一番上につけることで、多重定義を防ぐことが出来る。  
以下の例では、PlayerクラスとEnemyクラスで共通で使うVector2クラスを作り、それをどちらのファイルからも読み込むコンソールプログラムの例である。よって、Vector2クラスのpragma onceを外すと、コンパイルエラーが起きる。  

>Vector2.h

```cpp
#pragma once
#include <iostream>

class Vector2{
public:
	int x, y;
	Vector2(int _x, int _y) :
		x(_x),
		y(_y)
	{
	}
};
```

>Enemy.h

```cpp
#include <iostream>
#include "Vector2.h"

class Enemy{
public:
	Vector2 pos;

	Enemy(int x, int y) :
		pos(x, y)
	{
	}
};
```

>Player.h

```cpp
#include <iostream>
#include "Vector2.h"

class Player{
public:
	Vector2 pos;

	Player(int x, int y) :
		pos(x, y)
	{
	}
};
```

>main関数内

```cpp
#include <iostream>
#include "Player.h"
#include "Enemy.h"

int main(){

	Player player(100, 200);
	Enemy enemy(300, 400);

	std::cout << player.pos.x << ", " << player.pos.y << std::endl;
	std::cout << enemy.pos.x << ", " << enemy.pos.y << std::endl;

	return 0;
}
```



##まとめ
* クラスが他のクラスを実体としてメンバとして持つ、または他のクラスのメンバにアクセスするときは、他のクラスの定義がクラスの前方になければならない。　　
* 実際にゲームを作るときは、とりあえず#pragma onceをつけよう。#pragma onceは特に害はない。  



##演習問題(Siv3D)

1. 前回の演習問題で作ったプログラムのPlayerクラス, Enemyクラスのコンストラクタの初期化を今回教えた形式、メンバイニシャライザで書き直せ。  

1. 前回Siv3Dで作ったプロジェクト上で、Player.h, Player.cpp, Enemy.h, Enemy.cppを作り、PlayerクラスとEnemyクラスをファイル分けせよ。  

1. Siv3Dには2次元座標を表す、Vec2クラスがある。これを用いてPlayerクラス、Enemyクラスを書き換えよ。同時にdraw関数内の、Circleクラスの初期化もVec2クラスで行うよう書き換えよ。

> ヒント：Vec2クラスを用いた円描画の書き方  
> 座標:position(Vec2型), 半径:radius, 色:(r,g,b)  

```cpp
Circle(position, radius).draw(Color(r, g, b));
```


## Tips : クラスと構造体について
C++でのクラス(class)と構造体(struct)には、メンバーのデフォルトのアクセス属性が
privateであるか、publicであるかの違いしかない。Siv3Dには2次元座標を表す構造体、Vec2があるが、クラスと同じように扱える。（ちなみに、正確にはVec2はテンプレートクラスVector2Dでのテンプレート引数doubleを指定した、Vector2D<double>である)。それと、F12で定義を見に行けばわかると思うが、CircleもColorも構造体である。以後も、本資料では、構造体のこともクラスと呼ぶ場合がある。  


## Tips : クラス内定数
以下のようにすると、クラス内に定数を用意することができる。以下の例ではMyClassにint型のHoge定数、二次元座標を表すVec2クラスの定数を用意している。これらは、アクセス属性がpublicなので、外からもアクセスできるが、その場合は```Myclass::Hoge```のようにアクセスする。MyClass.cppで```const int MyClass::Hoge = 3;```のように書いているが、関数やコンストラクタ同様に、クラスの名前空間を```MyClass::```のように指定し忘れないように注意！また、MyClass.cppの数値を定義する部分では、staticは必要ない。  

> MyClass.h

```cpp
class MyClass {
public:
	static const int Hoge;
	static const Vec2 FirstPos;
	void func();
};
```

> MyClass.cpp

```cpp
const int MyClass::Hoge = 3;
const Vec2 MyClass::FirstPos(100, 200);

void MyClass::func(){
	std::cout << "aaa" << std::endl;
}
```