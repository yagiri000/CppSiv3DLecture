#C++講座前半課題

これまでやったことを用いて、添付資料のサンプルゲームと似たようなものを作れ。今回は練習として、GameManagerクラスが他のクラスを持つ形にすること。  


## ヒント
解答例は以下のようなクラス構成にした。  

GameManagerクラス(シングルトン。他のクラスを所持し、管理する）  
  |- Playerクラス  
  |- PlayerBulletManagerクラス(PlayerBulletクラスのvectorを所持)  
  |- EnemyManagerクラス(Enemyクラスのvectorを所持)  
  |- EnemyBulletManagerクラス(EnemyBulletクラスのvectorを所持)  
  |- EffectManagerクラス(Effectクラスのvectorを所持)  
  |- ScoreManagerクラス(スコアを管理するクラス)   


以下にVol1~Vol7までの演習問題の解答例を示す。演習問題(Siv3D)の解答例はファイルが多いので添付資料の中に配置した。  

## 解答例

## vol_0課題(Siv3D)

```cpp
# include <Siv3D.hpp>

void Main()
{
	// Fontクラスのインスタンスを生成。フォントサイズは18を指定
	const Font font(18);

	double x = 320.0;
	double y = 240.0;
	const double Speed = 5.0;

	while (System::Update())
	{
		// 上下左右キーで移動
		if (Input::KeyLeft.pressed) {
			x -= Speed;
		}
		if (Input::KeyRight.pressed) {
			x += Speed;
		}
		if (Input::KeyUp.pressed) {
			y -= Speed;
		}
		if (Input::KeyDown.pressed) {
			y += Speed;
		}

		// Zキーを押したフレームでランダムに移動
		if (Input::KeyZ.pressed) {
			x += Random(-30.0, 30.0);
		}

		// x, yに赤の円を描画
		Circle(x, y, 30.0).draw(Color(255, 0, 0));

		// 画面左上にx,yの数値を描画
		font(L"X:", x, L" Y:", y).draw();
	}
}
```

## vol_1課題1(コンソール)
```cpp
#include <iostream>

int main() {
	int input;
	std::cout << "整数値を入力してください：" << std::endl;
	std::cin >> input;//inputに入力された数値を代入
	std::cout << "入力された値は、" << input << "です。" << std::endl;

	return 0;
}

```
## vol_1課題2(コンソール)
```cpp
#include <iostream>

class Vector3 {
public:
	int x, y, z;

	// コンストラクタ
	Vector3(int x_, int y_, int z_) {
		x = x_;
		y = y_;
		z = z_;
	}

	// x, y, zの値を表示
	void show() {
		std::cout << x << "," << y << "," << z << std::endl;
	}

	// x * y * zの値を表示
	void showMultiple() {
		std::cout << x *y* z << std::endl;
	}
};

int main() {
	Vector3 vec3(1, 2, 3);
	vec3.show();
	vec3.showMultiple();

	return 0;
}
```

## vol_1課題(Siv3D)

> Main.cpp

```cpp
# include <Siv3D.hpp>


class Player {
public:
	double x, y;

	Player() {
		x = 320.0;
		y = 240.0;
	}
	void update() {
		const double speed = 5.0;
		// 上下左右キーで移動
		if (Input::KeyLeft.pressed) {
			x -= speed;
		}
		if (Input::KeyRight.pressed) {
			x += speed;
		}
		if (Input::KeyUp.pressed) {
			y -= speed;
		}
		if (Input::KeyDown.pressed) {
			y += speed;
		}
	}

	//自機（円）を描画
	void draw() {
		Circle(x, y, 30.0).draw(Color(0, 0, 255));
	}
};


class Enemy {
public:
	double x, y;

	Enemy(double _x, double _y) {
		x = _x;
		y = _y;
	}

	void update() {
		//下方向に移動
		y += 1.0;
	}

	//エネミー（円）を描画
	void draw() {
		Circle(x, y, 30.0).draw(Color(255, 0, 0));
	}
};


void Main()
{
	Player player; // プレイヤーのインスタンスを生成
	Enemy enemy(320, 100); // エネミーのインスタンスを生成

	while (System::Update())
	{
		player.update();
		enemy.update();

		player.draw();
		enemy.draw();
	}
}

```



## vol_2課題(Siv3D)
*以降の課題(Siv3D)は、C++Siv3D講座添付資料/解答例コード/を参照* 


## vol_3課題1(コンソール)

```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
	int x;

	MyClass(int _x) {
		x = _x;
	}

	void show() {
		std::cout << "xは：" << x << std::endl;
	}
};

int main() {
	std::vector<MyClass> vec;
	for (int i = 0; i < 10; i++) {
		vec.emplace_back(MyClass(rand() % 100));
	}

	for (int i = 0; i < (int)vec.size(); i++) {
		vec[i].show();
	}

	return 0;
}
```

## vol_3課題2(コンソール)
```cpp
#include <iostream>
#include <vector>

class Vector2 {
public:
	int x;
	int y;

	Vector2(int _x, int _y) {
		x = _x;
		y = _y;
	}
};

int main() {
	std::vector<Vector2> vec;
	for (int i = 0; i < 10; i++) {
		vec.emplace_back(Vector2(rand() % 100, rand() % 100));
	}

	for (int i = 0; i < (int)vec.size(); i++) {
		std::cout << vec[i].x << ", " << vec[i].y << std::endl;
	}

	return 0;
}
```

## vol_4課題(コンソール)

```cpp
#include <iostream>

void twiceRef(int &x) {
	x *= 2;
}


int main(void) {

	int num = 3;

	std::cout << num << std::endl;

	twiceRef(num);

	std::cout << num << std::endl;

	return 0;
}
```


## vol_5課題1(コンソール)

```cpp
#include <iostream>

int mySquare(int x) {
	std::cout << "int型のmySquareが呼ばれました" << std::endl;
	return x * x;
}

float mySquare(float x) {
	std::cout << "float型のmySquareが呼ばれました" << std::endl;
	return x * x;
}

double mySquare(double x) {
	std::cout << "double型のmySquareが呼ばれました" << std::endl;
	return x * x;
}


int main() {
	int x = 1;
	float y = 0.5;
	double z = 2.1;
	std::cout << mySquare(x) << std::endl;
	std::cout << mySquare(y) << std::endl;
	std::cout << mySquare(z) << std::endl;
}
```

## vol_5課題2(コンソール)

```cpp
#include <iostream>

class Vector2 {
public:
	int x, y;

	Vector2(int _x, int _y) :
		x(_x),
		y(_y)
	{
		std::cout << "引数ありのコンストラクタが呼ばれました" << std::endl;
	}

	Vector2() :
		x(0),
		y(0)
	{
		std::cout << "引数なしのコンストラクタが呼ばれました" << std::endl;
	}

	~Vector2() {
		std::cout << "デストラクタが呼ばれました" << std::endl;
	}
};

int main() {
	std::cout << "メイン関数に入りました" << std::endl;
	Vector2 point1;
	Vector2 point2(2, 3);
	std::cout << "メイン関数を抜けました" << std::endl;
}
```


## vol_6課題(コンソール)

```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
	int a;
	int b;

	MyClass(int _a, int bb) :
		a(_a),
		b(bb)
	{
	}
};

int main() {
	std::vector<MyClass> vec;

	for (int i = 0; i < 10; i++) {
		vec.emplace_back(MyClass(rand() % 10, rand() % 10));
	}

	for (auto iter = vec.begin(); iter < vec.end(); iter++) {
		std::cout << "(" << iter->a << ", " << iter->b << "), ";
	}

	std::cout << std::endl;

	auto iter = vec.begin();
	while (iter < vec.end()) {
		if (iter->a < iter->b) {
			iter = vec.erase(iter);
		}
		else iter++;
	}

	for (auto iter = vec.begin(); iter < vec.end(); iter++) {
		std::cout << "(" << iter->a << ", " << iter->b << "), ";
	}

	std::cout << std::endl;
}
```




## vol_7課題(コンソール)

> main.cpp

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

> Enemy.h

```cpp
#pragma once
#include <iostream>
#include "Manager.h"

class Enemy {
private:
	int x;
public:
	int getX();
	Enemy(int _x);
	void update();
	void showPlayerX();
};
```

> Enemy.cpp

```cpp
#include "Manager.h"

Enemy::Enemy(int _x) :
	x(_x)
{
}

int Enemy::getX() {
	return x;
}

void Enemy::update() {
	std::cout << "Enemy内のupdateが呼ばれました。xは:" << x << std::endl;
}

void Enemy::showPlayerX() {
	std::cout << "Enemy内のshowPlayerXが呼ばれました。" << std::endl;
	std::cout << "Playerのxは：" << manager.player.getX() << std::endl;
}
```

> Player.h

```cpp

#pragma once
#include <iostream>
#include "Manager.h"

class Player {
private:
	int x;
public:
	int getX();
	Player(int _x);
	void update();
	void showEnemyX();
};
```

> Player.cpp

```cpp
#include "Manager.h"

Player::Player(int _x) :
	x(_x)
{
}

int Player::getX() {
	return x;
}

void Player::update() {
	std::cout << "Player内のupdateが呼ばれました。xは:" << x << std::endl;
}

void Player::showEnemyX() {
	std::cout << "Player内のshowEnemyXが呼ばれました。" << std::endl;
	std::cout << "Enemyのxは：" << manager.enemy.getX() << std::endl;
}
```

> Manager.h

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
