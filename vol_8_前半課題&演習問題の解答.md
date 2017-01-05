#C++講座課題

これまでやったことを用いて、添付資料のサンプルゲームと似たようなものを作れ。  
今回は練習として、GameManagerクラスが他のクラスを持つ形にすること。  


## ヒント

GameManagerクラス(シングルトン。他のクラスを所持し、管理する）  
  |- Playerクラス  
  |- PlayerBulletManagerクラス(PlayerBulletクラスのvectorを所持)  
  |- EnemyManagerクラス(Enemyクラスのvectorを所持)  
  |- EnemyBulletManagerクラス(EnemyBulletクラスのvectorを所持)  
  |- EffectManagerクラス(Effectクラスのvectorを所持)  
  |- ScoreManagerクラス(スコアを管理するクラス)   

・以下はGameManagerクラスとは別に、シングルトンで作った。  
Inputクラス(キーの入力を管理するクラス)  


* 当たり判定  
弾と敵が当たったかを判定する関数が必要である。私は今回、弾も敵も自機もすべて円として考えた。  
よって2円が接触しているかどうかを返す関数のみを実装した。  

* エフェクト  
白い円のグラフィックが一瞬(8フレーム)で消えれば、爆風エフェクトっぽく見えるかもしれない。  
エフェクト管理クラスを作り、エフェクトの配列を作るといいかもしれない。  


以下にVol1~Vol7までの演習問題の解答例を示す。Vol7演習問題(DXライブラリ)の解答例はファイルが多いので添付資料の中に配置した。  

## 解答例

## vol_0課題(DXライブラリ)

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

	double x = 400.0;
	double y = 300.0;
	const double v = 4.5;

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新
		
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

		// Zキーを押したフレームにランダム移動
		if (keyState[KEY_INPUT_Z] == 1) {
			x += GetRand(100);
		}

		// x, y に円を描画
		DrawCircle(x, y, 20, GetColor(255, 0, 0));

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
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
	void Show() {
		std::cout << x << "," << y << "," << z << std::endl;
	}

	// x * y * zの値を表示
	void ShowMultiple() {
		std::cout << x *y* z << std::endl;
	}
};

int main() {
	Vector3 vec3(1, 2, 3);
	vec3.Show();
	vec3.ShowMultiple();

	return 0;
}
```

## vol_1課題(DXライブラリ)
サンプルプロジェクトのmain.cppを以下に書き換え

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

	double x = 400.0;
	double y = 300.0;
	const double v = 4.5;

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新
		
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

		// Zキーを押したフレームにランダム移動
		if (keyState[KEY_INPUT_Z] == 1) {
			x += GetRand(100);
		}

		// x, y に円を描画
		DrawCircle(x, y, 20, GetColor(255, 0, 0));

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```




## vol_1課題(DXライブラリ)
サンプルプロジェクトのmain.cppを以下に書き換え

```cpp
#include <DxLib.h>
#include <iostream>
#include "myglobal.h"

class Player {
public:
	double x, y, v;

	Player() {
		x = 400;
		y = 400;
		v = 4.5;
	}
	void Update() {
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

	//自機（円）を描画
	void Draw() {
		DrawCircle(x, y, 10, GetColor(0, 0, 255), 1);
	}
};

class Enemy {
public:
	double x, y, vy;

	Enemy(double x_, double y_) {
		x = x_;
		y = y_;
		vy = 0.5;
	}

	void Update() {
		y += vy;
	}
	//敵（円）を描画
	void Draw() {
		DrawCircle(x, y, 10, GetColor(255, 0, 0), 1);
	}
};

int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	Player player;
	Enemy enemy(400, 100);

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		player.Update();
		enemy.Update();

		player.Draw();
		enemy.Draw();

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```



## vol_2課題(DXライブラリ)

main.cpp

```cpp
#include <DxLib.h>
#include <iostream>
#include "myglobal.h"
#include "Player.h"
#include "Enemy.h"

int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	Player player;
	Enemy enemy(400, 100);

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		player.Update();
		enemy.Update();

		player.Draw();
		enemy.Draw();

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```

Player.h

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

Player.cpp

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
	DrawCircle(x, y, 10, GetColor(0, 0, 255), 1);
}

```

Enemy.h

```cpp
#pragma once

class Enemy {
public:
	double x, y, vy;
	Enemy(double x_, double y_);
	void Update();
	void Draw();
};
```

Enemy.cpp

```cpp
#include <DxLib.h>
#include "Enemy.h"

Enemy::Enemy(double x_, double y_) {
	x = x_;
	y = y_;
	vy = 0.5;
}

void Enemy::Update() {
	y += vy;
}

void Enemy::Draw() {
	DrawCircle(x, y, 10, GetColor(255, 0, 0), 1);
}

```



## vol_3課題1(コンソール)

```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
	int x;

	MyClass(int xx) {
		x = xx;
	}

	void Show() {
		std::cout << "xは：" << x << std::endl;
	}
};

int main() {
	std::vector<MyClass> vec;
	for (int i = 0; i < 10; i++) {
		vec.push_back(MyClass(rand() % 100));
	}

	for (int i = 0; i < (int)vec.size(); i++) {
		vec[i].Show();
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

	Vector2(int xx, int yy) {
		x = xx;
		y = yy;
	}
};

int main() {
	std::vector<Vector2> vec;
	for (int i = 0; i < 10; i++) {
		vec.push_back(Vector2(rand() % 100, rand() % 100));
	}

	for (int i = 0; i < (int)vec.size(); i++) {
		std::cout << vec[i].x << ", " << vec[i].y << std::endl;
	}

	return 0;
}
```

## vol_3課題(DXライブラリ)

EnemyManager.h

```cpp
#pragma once
#include "Enemy.h"
#include <vector>

class EnemyManager {
public:
	std::vector<Enemy> enemies;
	void Update();
	void Draw();
	void AddEnemy(double x, double y);
};
```

EnemyManager.cpp

```cpp
#include "EnemyManager.h"

void EnemyManager::Update()
{
	for (int i = 0; i < enemies.size(); i++) {
		enemies[i].Update();
	}
}

void EnemyManager::Draw()
{
	for (int i = 0; i < enemies.size(); i++) {
		enemies[i].Draw();
	}
}

void EnemyManager::AddEnemy(double x, double y)
{
	enemies.push_back(Enemy(x, y));
}
```

main.cpp

```cpp
#include <DxLib.h>
#include <iostream>
#include "myglobal.h"
#include "EnemyManager.h"

int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	EnemyManager enemyManager;

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		if (keyState[KEY_INPUT_SPACE]) {
			enemyManager.AddEnemy(GetRand(800), GetRand(600));
		}

		enemyManager.Update();
		enemyManager.Draw();

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```


```cpp
```


```cpp
```


## vol_4課題1(コンソール)

```cpp
#include <iostream>
#include <string>

int main() {
	std::string str;
	std::string temp;
	while (!(temp == "show")) {
		std::cin >> temp;
		str += temp;
	}
	std::cout << str;
}

```


## vol_4課題2(コンソール)

```cpp
#include <iostream>
#include <string>

void function(std::string& str) {
	str += "その点トッポってすげぇよな、最後までチョコたっぷりだもん。";
}

int main() {
	std::string str;
	std::cin >> str;
	function(str);
	std::cout << str << std::endl;
}
```

## vol_4課題(DXライブラリ)

> main.cpp

```cpp
#include <DxLib.h>
#include <iostream>
#include <string>
#include "myglobal.h"
#include "Player.h"
#include "Enemy.h"


int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	Player player;
	Enemy enemy(400, 300);
	enemy.SetPlayerPointer(&player);

	std::string str = "aaa";


	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		player.Update();
		enemy.Update();

		// かわいいを追加
		if (keyState[KEY_INPUT_X] == 1) {
			str += "かわいい";
		}

		player.Draw();
		enemy.Draw();

		// 文字列を描画
		DrawFormatStringToHandle(30, 30, GetColor(255, 255, 255), FontHandle, str.c_str());

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```

> Enemy.h

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
	void SetPlayerPointer(Player * ptr);
};

```


> Enemy.cpp

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
	if (x < player->x) {
		vx = 1.0;
	}
	else {
		vx = -1.0;
	}
	if (y < player->y) {
		vy = 1.0;
	}
	else {
		vy = -1.0;
	}

	x += vx;
	y += vy;
}

void Enemy::Draw() {
	DrawCircle(x, y, 24, GetColor(255, 0, 0), 1);
}

void Enemy::SetPlayerPointer(Player *ptr) {
	player = ptr;
}
```


## vol_5課題1(コンソール)

```cpp
#include <iostream>

int MySquare(int x) {
	std::cout << "int型のMySquareが呼ばれました" << std::endl;
	return x * x;
}

float MySquare(float x) {
	std::cout << "float型のMySquareが呼ばれました" << std::endl;
	return x * x;
}

double MySquare(double x) {
	std::cout << "double型のMySquareが呼ばれました" << std::endl;
	return x * x;
}


int main() {
	int x = 1;
	float y = 0.5;
	double z = 2.1;
	std::cout << MySquare(x) << std::endl;
	std::cout << MySquare(y) << std::endl;
	std::cout << MySquare(z) << std::endl;
}
```

## vol_5課題2(コンソール)

```cpp
#include <iostream>

class Vector2 {
public:
	int x, y;

	Vector2(int xx, int yy) :
		x(xx),
		y(yy)
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

	MyClass(int aa, int bb) :
		a(aa),
		b(bb)
	{
	}
};

int main() {
	std::vector<MyClass> vec;

	for (int i = 0; i < 10; i++) {
		vec.push_back(MyClass(rand() % 10, rand() % 10));
	}

	for (auto itr = vec.begin(); itr < vec.end(); itr++) {
		std::cout << "(" << itr->a << ", " << itr->b << "), ";
	}

	std::cout << std::endl;

	auto itr = vec.begin();
	while (itr < vec.end()) {
		if (itr->a < itr->b) {
			itr = vec.erase(itr);
		}
		else itr++;
	}

	for (auto itr = vec.begin(); itr < vec.end(); itr++) {
		std::cout << "(" << itr->a << ", " << itr->b << "), ";
	}

	std::cout << std::endl;
}
```

## vol_6課題(DXライブラリ)

```cpp
#include <DxLib.h>
#include <iostream>
#include <vector>
#include "myglobal.h"
#include "Enemy.h"


int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	std::vector<Enemy> enemies;

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		// 敵を生成
		if (keyState[KEY_INPUT_Z] == 1) {
			enemies.push_back(Enemy(GetRand(800), GetRand(600)));
		}

		// 敵全てのアップデート
		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			i->Update();
		}

		// 敵削除
		auto it = enemies.begin();
		while (it != enemies.end()) {
			if (it->x > 800.0 || it->x < 0.0 || it->y > 600.0 || it->y < 0.0) {//画面外に出ているか確認
				it = enemies.erase(it);
			}
			else {
				it++;
			}
		}

		// 敵の描画
		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			i->Draw();
		}

		// 敵の数を表示
		DrawFormatStringToHandle(30, 30, GetColor(255, 255, 255), FontHandle, "Enemy:%d", enemies.size());

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```





## vol_7課題(コンソール)

> main.cpp

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

> Enemy.h

```cpp
#pragma once
#include <iostream>
#include "Manager.h"

class Enemy {
private:
	int x;
public:
	int GetX();
	Enemy(int xx);
	void Update();
	void ShowPlayerX();
};
```

> Enemy.cpp

```cpp
#include "Manager.h"

Enemy::Enemy(int xx) :
	x(xx)
{
}

int Enemy::GetX() {
	return x;
}

void Enemy::Update() {
	std::cout << "Enemy内のUpdateが呼ばれました。xは:" << x << std::endl;
}

void Enemy::ShowPlayerX() {
	std::cout << "Enemy内のShowPlayerXが呼ばれました。" << std::endl;
	std::cout << "Playerのxは：" << manager.player.GetX() << std::endl;
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
	int GetX();
	Player(int xx);
	void Update();
	void ShowEnemyX();
};
```

> Player.cpp

```cpp
#include "Manager.h"

Player::Player(int xx) :
	x(xx)
{
}

int Player::GetX() {
	return x;
}

void Player::Update() {
	std::cout << "Player内のUpdateが呼ばれました。xは:" << x << std::endl;
}

void Player::ShowEnemyX() {
	std::cout << "Player内のShowEnemyXが呼ばれました。" << std::endl;
	std::cout << "Enemyのxは：" << manager.enemy.GetX() << std::endl;
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


## vol_7課題(DXライブラリ)
C++DxLib講座添付資料/解答例コード/Vol7_演習問題(DxLib)解答例　参照