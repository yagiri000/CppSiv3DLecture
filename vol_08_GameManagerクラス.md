#cpp講座資料

##GameManagerクラス
実際ゲームを作る時、PlayerやEnemyの配列、Bulletの配列、Effectの配列等をメンバに持ち、それらを管理するManagerクラスを作ると便利である。  
今回は、PlayerとEnemyを持つManagerクラスの実体をグローバル変数にすることで、EnemyがPlayerが相互にアクセスする例を示す。
(前回は、EnemyクラスがPlayerへのポインタを持つことで、Playerクラスにアクセスすることができたが、今回はEnemyクラスがPlayerクラスへのポインタを持つ必要はない)　　
PlayerがEnemyの情報に、EnemyがPlayerの情報に相互にアクセス出来るようにするため、上手くファイルをインクルードしなければならない。  
以下の例では、PlayerはshowEnemyX関数でEnemyのxにアクセスし、EnemyはshowPlayerX関数でPlayerのxにアクセスしている。  


>Player.h

```cpp
#pragma once
#include <iostream>
#include "Manager.h"

class Player {
public:
	int x;
	Player(int _x);
	void update();
	void showEnemyX();
};
```

>Player.cpp

```cpp
#include "Manager.h"

Player::Player(int _x) :
	x(_x)
{
}

void Player::update() {
	std::cout << "Player内のupdateが呼ばれました。xは:" << x << std::endl;
}

void Player::showEnemyX() {
	std::cout << "Player内のshowEnemyXが呼ばれました。" << std::endl;
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
	Enemy(int _x);
	void update();
	void showPlayerX();
};
```

>Enemy.cpp

```cpp
#include "Manager.h"

Enemy::Enemy(int _x) :
	x(_x)
{
}

void Enemy::update() {
	std::cout << "Enemy内のupdateが呼ばれました。xは:" << x << std::endl;
}

void Enemy::showPlayerX() {
	std::cout << "Enemy内のshowPlayerXが呼ばれました。" << std::endl;
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

	manager.player.update();
	manager.enemy.update();

	manager.player.showEnemyX();
	manager.enemy.showPlayerX();

	return 0;
}
```

## 演習問題(コンソール)  

* 上記のPlayerクラスのx, Enemyクラスのxはpublicメンバで、値を取得するだけでなく直接書き換えることが可能であり、危険である。この2つをprivateメンバにし、外から値が取得できるようにgetX()関数を実装せよ。また、それに伴いshowPlayerX, showEnemyX関数の中身も書き換えよ。（今回はprivateの使用例を紹介するためgetter関数を用意するが、煩雑になるため、本資料ではこの後も基本的にprivateは使用しない。）  


## 演習問題(Siv3D)

1. GameManagerクラスを作れ。  
GameManagerクラスは、PlayerやEnemyManager(Enemyのvectorを管理するクラス)をメンバに持ち、それらを管理するクラスである。    
GameManagerのインスタンスをグローバル変数にすることで、GameManagerを介してPlayerやEnemyの情報にアクセスできるようにせよ。  
GameManagerクラスのupdateを呼べば、PlayerとすべてのEnemyのupdateが呼ばれるようにせよ。  
EnemyクラスがPlayerクラスへのポインタを持っていた場合、それを消して、Playerクラスへのアクセスはグローバル空間に存在するGameManagerクラスのインスタンスを介して行うようにせよ。  

1. Enemyが自機を追うようにせよ。

1. Enemyクラス内にint型のkind変数を作り、kind変数によって敵が多様な動きをするようにせよ。  
(kind==1なら自機に直進、kind==2ならsin軌道、kind==3なら出た位置で円運動…など)  
kindの値は敵の生成時にGetRand()などで適当に決めて良い。add関数の引数で指定できても良い。    
ちなみに、GetRand(3)とすると、0,1,2,3のどれかが返ってくる。0,1,2でないことに注意。

1. kindによって敵の色が変わるようにせよ。

1. kindをenum型にせよ。クラス内で定義されたenumは、クラス外部からは(クラス名)::(要素の名前)で	enumを指定できる。


## ヒント：enum型    
クラス内でenumを定義しコンストラクタで指定する例。  
  
```cpp
#include <iostream>

class Hoge {
public:
	enum Kind {
		A,
		B,
		C,
	};

	Kind kind;

	//コンストラクタ　kindを指定
	Hoge(Kind _kind) :
	kind(_kind)
	{
	}
};

int main(void) {
	// コンストラクタでkindを指定
	Hoge ins(Hoge::B);

	// 出力。内部的には整数（0から始まる）
	std::cout << ins.kind << std::endl;

	return 0;
}
```
