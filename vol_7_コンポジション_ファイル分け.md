#cpp講座資料

実際にゲームを作る時、ファイル分けをすることが必要になる。  
今回はクラスの内容を.hと.cppに分け、それが必要なケースと、なぜ必要になるのかを解説する。      



##コンポジション
クラスをクラスのメンバにすることが出来る。  
以下の例では、Vector2クラスをまず定義し、PlayerクラスでVector2クラスのメンバを持っている。  

```cpp
#include <iostream>

class Vector2{
public:
	int x, y;
	Vector2(int xx, int yy) :
		x(xx),
		y(yy)
	{
	}
};

class Player{
public:
	Vector2 pos;
	int hp;

	Player(int x, int y, int hp_) :
		pos(x, y),
		hp(hp_)
	{
	}
};


int main(void){
	Player player(100, 200, 64);

	std::cout << "x:" << player.pos.x << ", y:" << player.pos.y << std::endl;
	std::cout << "hp:" << player.hp << std::endl;

	return 0;
}

```

コンストラクタは以下のように書く。引数を受け渡すと考えれば良い。  
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

	Hoge(int xx):
		x(xx)
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

	Piyo(int xx, int yy):
		Hoge_(xx),
		y(yy)
	{
		std::cout << "Piyoのコンストラクタが呼ばれました" << std::endl;
	}

	~Piyo(){
		std::cout << "Piyoのデストラクタが呼ばれました" << std::endl;
	}
};


int main(void){

	Piyo Piyo_(100, 200);

	std::cout << Piyo_.Hoge_.x << " " << Piyo_.y << std::endl;

	return 0;
}
```


##インクルード
\#includeを使うと自分が定義したヘッダーをインクルードできる。インクルードすると、その部分にヘッダーの中身のコードが展開される。

##\#pragma once
\#pragma onceを使うと一度読み込まれたヘッダーは読み込まれないようになる。複数のファイルから読み込まれるヘッダーの一番上につけることで、多重定義を防ぐことが出来る。  
以下の例では、PlayerクラスとEnemyクラスで共通で使うVector2クラスを作り、それをどちらのファイルからも読み込んでいる。  
Vector2クラスのpragma onceを外すと、コンパイルエラーが起きる。  

>Vector2.h

```cpp
#pragma once
#include <iostream>

class Vector2{
public:
	int x, y;
	Vector2(int xx, int yy) :
		x(xx),
		y(yy)
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
	Vector2 pt;

	Enemy(int x, int y) :
		pt(x, y)
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
	Vector2 pt;

	Player(int x, int y) :
		pt(x, y)
	{
	}
};
```

>main関数内

```cpp
#include <iostream>
#include "Player.h"
#include "Enemy.h"


int main(void){

	Player player(100, 200);
	Enemy enemy(300, 400);

	std::cout << player.pt.x << ", " << player.pt.y << std::endl;
	std::cout << enemy.pt.x << ", " << enemy.pt.y << std::endl;

	return 0;
}
```



##まとめ
* クラスが他のクラスを実体としてメンバとして持つ、または他のクラスのメンバにアクセスするときは、他のクラスの定義がクラスの前方（コードの上のほう）になければならない。　　
* 実際にゲームを作るときは、とりあえず#pragma onceをつけよう。#pragma onceは特に害はない。  



##GameManagerクラス
実際ゲームを作る時、PlayerやEnemyの配列、Bulletの配列、Effectの配列等をメンバに持ち、それらを管理するManagerクラスを作ると便利である。  
今回は、PlayerとEnemyを持つManagerクラスの実体をグローバル変数にすることで、EnemyがPlayerが相互にアクセスする例を示す。
(前回は、EnemyクラスがPlayerへのポインタを持つことで、Playerクラスにアクセスすることができたが、今回はEnemyクラスがPlayerクラスへのポインタを持つ必要はない)　　
PlayerがEnemyの情報に、EnemyがPlayerの情報に相互にアクセス出来るようにするため、上手くファイルをインクルードしなければならない。  
以下の例では、PlayerはShowEnemyX関数でEnemyのxにアクセスし、EnemyはShowPlayerX関数でPlayerのxにアクセスしている。  


>Player.h

```cpp
#pragma once
#include <iostream>
#include "Manager.h"

class Player {
public:
	int x;
	Player(int xx);
	void Update();
	void ShowEnemyX();
};
```

>Player.cpp

```cpp
#include "Manager.h"

Player::Player(int xx) :
	x(xx)
{
}

void Player::Update() {
	std::cout << "Player内のUpdateが呼ばれました。xは:" << x << std::endl;
}

void Player::ShowEnemyX() {
	std::cout << "Player内のShowEnemyXが呼ばれました。" << std::endl;
	std::cout << "Enemyのxは：" << manager.enemy.x << std::endl;
}
```

>Enemy.h

```cpp
#pragma once
#include <iostream>
#include "Manager.h"

class Enemy {
public:
	int x;
	Enemy(int xx);
	void Update();
	void ShowPlayerX();
};
```

>Enemy.cpp

```cpp
#include "Manager.h"

Enemy::Enemy(int xx) :
	x(xx)
{
}

void Enemy::Update() {
	std::cout << "Enemy内のUpdateが呼ばれました。xは:" << x << std::endl;
}

void Enemy::ShowPlayerX() {
	std::cout << "Enemy内のShowPlayerXが呼ばれました。" << std::endl;
	std::cout << "Playerのxは：" << manager.player.x << std::endl;
}
```


>Manager.h

```cpp
#pragma once
#include "Player.h"
#include "Enemy.h"

class Manager {
public:
	Player player;
	Enemy enemy;

	Manager();
};

extern Manager manager;
```

> Manager.cpp

```cpp
#include "Manager.h"

Manager::Manager() :
	player(100),
	enemy(200)
{
};

Manager manager;
```

>main.cpp

```cpp
#include <iostream>
#include "Manager.h"

int main() {

	manager.player.Update();
	manager.enemy.Update();

	manager.player.ShowEnemyX();
	manager.enemy.ShowPlayerX();

	return 0;
}
```

##演習問題  

* 上記のPlayerクラスのx, Enemyクラスのxはpublicメンバで、値を取得するだけでなく直接書き換えることが可能であり、危険である。この2つをprivateメンバにし、外から値が取得できるようにGetX()関数を実装せよ。また、それに伴いShowPlayerX, ShowEnemyX関数の中身も書き換えよ。（今回はprivateの使用例を紹介するためgetter関数を用意したが、煩雑になるため、本資料ではこの後も基本的にprivateは使用しない。）  


##演習問題(DXライブラリ)

1. GameManagerクラスを作れ。  
GameManagerクラスは、PlayerやEnemyManager(Enemyのvectorを管理するクラス)をメンバに持ち、それらを管理するクラスである。    
GameManagerのインスタンスをグローバル変数にすることで、GameManagerを介してPlayerやEnemyの情報にアクセスできるようにせよ。  
GameManagerクラスのUpdateを呼べば、PlayerとすべてのEnemyのUpdateが呼ばれるようにせよ。  
EnemyクラスがPlayerクラスへのポインタを持っていた場合、それを消して、Playerクラスへのアクセスはグローバル空間に存在するGameManagerクラスのインスタンスを介して行うようにせよ。  

1. Enemyが自機を追うようにせよ。

1. Enemyクラス内にint型のmode変数を作り、mode変数によって敵が多様な動きをするようにせよ。  
(mode==1なら自機に直進、mode==2ならsin軌道、mode==3なら出た位置で円運動…など)  
modeの値は敵の生成時にGetRand()などで適当に決めて良い。AddEnemy関数の引数で指定できても良い。    
ちなみに、GetRand(3)とすると、0,1,2,3のどれかが返ってくる。0,1,2でないことに注意。

1. modeによって敵の色が変わるようにせよ。

1. modeをenum型にせよ。クラス内で定義されたenumは、クラス外部からは(クラス名)::(要素の名前)で	enumを指定できる。


##ヒント：enum型    
クラス内でenumを定義し使用する例。  
Hogeクラスを用意し、enumのPiyoである変数modeを用意し、クラス外部からmodeを変更して出力していっている。
  
```cpp
#include <iostream>

class Hoge{
public:
	enum Piyo{
		aaa,
		bbb,
		ccc,
	};

	Piyo mode;

	//コンストラクタ　modeの初期値をaaaに
	Hoge(){
		mode = aaa;
	}

	//modeをセットする関数
	void SetMode(Piyo p){
		mode = p;
	}
};

int main(void){
	Hoge ins;

	//出力。内部的には整数（0から始まる）
	std::cout << ins.mode << std::endl;

	//クラス外からアクセスするには「クラス名::項目」とする。
	ins.mode = Hoge::bbb;
	//出力
	std::cout << ins.mode << std::endl;

	//関数を用いてセット
	ins.SetMode(Hoge::ccc);
	std::cout << ins.mode << std::endl;

	return 0;
}
```
