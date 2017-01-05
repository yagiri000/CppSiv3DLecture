#C++講座 後半課題

前半の課題をポリモーフィズムを用いて書き直せ。    
練習として、敵の基底クラスへのスマートポインタのvectorに派生クラスを入れ、敵の削除にはremove_ifとラムダ式を使うこと。    

以下にVol9~Vol12の解答例を示す。

## vol_9課題(コンソール)

> main.cpp を以下のように書き換え

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

	std::vector<Enemy*> enemies;

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		// 敵を生成
		if (keyState[KEY_INPUT_Z] == 1) {
			enemies.emplace_back(new Enemy(GetRand(800), GetRand(200)));
		}

		// 敵削除
		auto it = enemies.begin();
		while (it != enemies.end()) {
			if ((*it)->x > 800.0 || (*it)->x < 0.0 || (*it)->y > 600.0 || (*it)->y < 0.0) {//画面外に出ているか確認
				delete *it;
				it = enemies.erase(it);
			}
			else {
				it++;
			}
		}

		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			(*i)->Update();
		}

		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			(*i)->Draw();
		}

		// 敵の数を表示
		DrawFormatString(30, 30, GetColor(255, 255, 255), "Enemy : %d", enemies.size());

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```


## vol_10課題(コンソール)

```cpp
#include <iostream>
#include <vector>

class IAnimal {
public:
	double weight;//重さ

	IAnimal(int w) : weight(w) {

	}

	virtual void Talk() {
		std::cout << "基底クラスのTalk関数が呼ばれました。 重さは:" << weight << std::endl;
	}
};

class Dog : public IAnimal {
public:
	Dog(int w) : IAnimal(w) {

	}

	void Talk() {
		std::cout << "わんわん　重さは:" << weight << std::endl;
	}
};

class Yojo : public IAnimal {
public:
	Yojo(int w) : IAnimal(w) {

	}

	void Talk() {
		std::cout << "ふぇぇ…　重さは:" << weight << std::endl;
	}
};

class Cat : public IAnimal {
public:
	Cat(int w) : IAnimal(w) {

	}
	
	void Talk() {
		std::cout << "にゃー　重さは:" << weight << std::endl;
	}
};

int main() {
	std::vector<IAnimal*> animals;
	animals.emplace_back(new Dog(10));
	animals.emplace_back(new Yojo(20));
	animals.emplace_back(new Cat(30));

	for (auto i = animals.begin(); i < animals.end(); i++) {
		(*i)->Talk();
	}

	return 0;
}
```


## vol_10課題(DXライブラリ)
解答例に添付


## vol_11課題(DXライブラリ)

> main.cppを以下のように書き換え

```cpp
#include <DxLib.h>
#include <iostream>
#include <vector>
#include <memory>

#include "myglobal.h"
#include "Enemy.h"

int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	std::vector<std::shared_ptr<IEnemy>> enemies;

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		// 敵を生成
		if (keyState[KEY_INPUT_A] == 1) {
			enemies.emplace_back(std::make_shared<EnemyA>(GetRand(800), GetRand(200)));
		}
		if (keyState[KEY_INPUT_S] == 1) {
			enemies.emplace_back(std::make_shared<EnemyB>(GetRand(800), GetRand(200)));
		}
		if (keyState[KEY_INPUT_D] == 1) {
			enemies.emplace_back(std::make_shared<EnemyC>(GetRand(800), GetRand(200)));
		}

		// 敵削除
		auto it = enemies.begin();
		while (it != enemies.end()) {
			if ((*it)->x > 800.0 || (*it)->x < 0.0 || (*it)->y > 600.0 || (*it)->y < 0.0) {//画面外に出ているか確認
				it = enemies.erase(it);
			}
			else {
				it++;
			}
		}

		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			(*i)->Update();
		}

		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			(*i)->Draw();
		}

		// 敵の数を表示
		DrawFormatString(30, 30, GetColor(255, 255, 255), "敵の数 : %d", enemies.size());

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```

## vol_12課題1(コンソール)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <memory>

class MyClass {
public:
	int a;

	MyClass(int aa) :
		a(aa)
	{
	}
};


// aが5以下だとtrue, そうでない場合はfalseを返す叙述関数
bool IsMin5(const MyClass& ins) {
	return ins.a <= 5;
}

int main() {

	std::vector<MyClass> vec;

	for (int i = 0; i < 10; i++) {
		vec.emplace_back(MyClass(rand() % 10));
	}

	// vectorの中身を表示
	for (auto i = vec.begin(); i < vec.end(); i++) {
		std::cout << i->a << " ";
	}
	std::cout << std::endl;

	// vecの中から3の倍数を後ろに詰める
	auto rmvIter = std::remove_if(vec.begin(), vec.end(), IsMin5);

	// 実際に削除
	vec.erase(rmvIter, vec.end());

	// vectorの中身を表示
	for (auto i = vec.begin(); i < vec.end(); i++) {
		std::cout << i->a << " ";
	}
	std::cout << std::endl;

	return 0;
}
```

## vol_12課題1(コンソール)

```cpp
#include <iostream>

int main() {

	auto twice = [](int x) {
		return x * 2;
	};

	int num = 3;
	
	std::cout << twice(num) << std::endl;

	return 0;
}
```

## vol_12課題2(コンソール)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
	std::vector<int> vec;
	
	// vectorに0から9の数を入れる
	for (int i = 0; i < 10; i++) {
		vec.emplace_back(i);
	}

	//表示
	for (auto i = vec.begin(); i < vec.end(); ++i) {
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	// 2の倍数を後ろに詰め、削除
	auto rmvIter = std::remove_if(vec.begin(), vec.end(), 
		[](int i) {return i % 2 == 0; }
	);
	vec.erase(rmvIter, vec.end());

	//表示
	for (auto i = vec.begin(); i < vec.end(); ++i) {
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	return 0;
}
```

## vol_12課題3(コンソール)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <memory>

class MyClass {
public:
	int a;

	MyClass(int aa) :
		a(aa)
	{
	}
};


int main() {

	std::vector<MyClass> vec;

	for (int i = 0; i < 10; i++) {
		vec.emplace_back(MyClass(rand() % 10));
	}

	// vectorの中身を表示
	for (auto i = vec.begin(); i < vec.end(); i++) {
		std::cout << i->a << " ";
	}
	std::cout << std::endl;

	// vecの中から5以下のaを持つ要素を後ろに詰める
	auto rmvIter = std::remove_if(vec.begin(), vec.end(), [](const MyClass& ins) {
		return ins.a <= 5;
	});

	// 実際に削除
	vec.erase(rmvIter, vec.end());

	// vectorの中身を表示
	for (auto i = vec.begin(); i < vec.end(); i++) {
		std::cout << i->a << " ";
	}
	std::cout << std::endl;

	return 0;
}
```

## vol_12課題(DXライブラリ)

> main.cpp

```cpp
#include <DxLib.h>
#include <iostream>
#include <vector>
#include <memory>
#include <algorithm>

#include "myglobal.h"
#include "Enemy.h"

int WINAPI WinMain( HINSTANCE hInstance , HINSTANCE hPrevInstance , LPSTR lpCmdLine , int nCmdShow )
{
	ChangeWindowMode( TRUE );//非全画面にセット
	SetGraphMode( 800 , 600 , 32 );//画面サイズ指定
	SetOutApplicationLogValidFlag( FALSE ) ;//Log.txtを生成しないように設定
	if(DxLib_Init() == 1){return -1;}//初期化に失敗時にエラーを吐かせて終了

	FontHandle = CreateFontToHandle( "Segoe UI" , 20 , 5 ,DX_FONTTYPE_ANTIALIASING_4X4) ;//フォントを読み込み

	std::vector<std::shared_ptr<IEnemy>> enemies;

	while( ProcessMessage()==0 )
	{
		ClearDrawScreen();//裏画面消す
		SetDrawScreen( DX_SCREEN_BACK ) ;//描画先を裏画面に

		GetMousePoint( &mousex, &mousey ); //マウス座標更新
		KeyUpdate();//(自作関数)キー更新

		// 敵を生成
		if (keyState[KEY_INPUT_A] == 1) {
			enemies.emplace_back(std::make_shared<EnemyA>(GetRand(800), GetRand(200)));
		}
		if (keyState[KEY_INPUT_S] == 1) {
			enemies.emplace_back(std::make_shared<EnemyB>(GetRand(800), GetRand(200)));
		}
		if (keyState[KEY_INPUT_D] == 1) {
			enemies.emplace_back(std::make_shared<EnemyC>(GetRand(800), GetRand(200)));
		}

		// 敵削除
		auto rmvIter = std::remove_if(enemies.begin(), enemies.end(), 
			[](const std::shared_ptr<IEnemy>& ptr) {
			return ptr->x > 800.0 || ptr->x < 0.0 || ptr->y > 600.0 || ptr->y < 0.0;
		});

		enemies.erase(rmvIter, enemies.end());

		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			(*i)->Update();
		}

		for (auto i = enemies.begin(); i < enemies.end(); i++) {
			(*i)->Draw();
		}

		// 敵の数を表示
		DrawFormatString(30, 30, GetColor(255, 255, 255), "敵の数 : %d", enemies.size());

		ScreenFlip();//裏画面を表画面にコピー
	}

	DxLib_End();
	return 0;
}
```
