#C++Siv3D講座資料

今回はvectorの要素を削除する方法を学ぶ。Siv3Dを用いた演習では、画面外に出た敵を削除する。
C++にはSTLという便利なライブラリが標準でついている。STLにはコンテナがあり、コンテナには便利な配列やリスト構造などがはじめから用意されている。vectorはコンテナの一種である。

##イテレータ

ポインタを抽象化してstd::vectorや他のコンテナでも似た形で使えるようにしたイテレータというものが存在する。  
ポインタが変数をさすのに対して、イテレータはvectorの要素をさす。  
begin()関数は、配列の先頭へのイテレータを返す関数、end()関数は、配列の最後の要素の次へのイテレータを返す関数。  
下の例では、これらを用いてfor文を回している。

```cpp
#include <iostream>
#include <vector>

int main(){

	std::vector<int> vec;
 
	// 10個の要素を追加していく
	for(int i = 0; i < 10; i++ ){
		vec.emplace_back(rand()%100);
	}
 
	//イテレータを用いて出力
	for(std::vector<int>::iterator iter =vec.begin(); iter !=vec.end(); ++iter){
		std::cout << *iter << " ";
	}
	std::cout << std::endl;
	
	
	//begin()関数は、配列の先頭へのイテレータを返す関数
	std::vector<int>::iterator iter2 = vec.begin();
	std::cout << *iter2 << std::endl;

	//イテレータはポインタと同じく+することで指す要素を進められる
	//はじめの要素を指すイテレータを4つ進めたので、5番目の要素が出力される
	iter2 += 4;
	std::cout << *iter2 << std::endl;
	
	return 0;
}
```

std::vector&lt;int&gt;::iteratorは長いので、C++の機能である型推論を使い表記を省略できる。
型名の代わりに、autoと書くことで、コンパイラが型を推測してくれる。

```cpp
#include <iostream>
#include <vector>

int main(){

	std::vector<int> vec;
 
	// 10個の要素を追加していく
	for(int i = 0; i < 10; i++ ){
		vec.emplace_back(i);
	}
 
	//イテレータを用いて出力
	for(auto iter =vec.begin(); iter !=vec.end(); ++iter){
		std::cout << *iter << " ";
	}
	std::cout << std::endl;
	
	return 0;
}
```

##クラスのvectorのイテレータ
MyClassにxという要素があったとする。「MyClassのvectorのイテレータ」からそれが指すMyClassのxにアクセスするには、  
イテレータをiterとすると、下のように書けばよい。  

```cpp
iter->x
```

>Vector2クラスのvectorへのイテレータを使ってVector2クラスのx, yを出力

```cpp
#include <iostream>
#include <vector>

class Vector2{
public:
	int x;
	int y;

	Vector2(int _x, int _y) :
		x(_x),
		y(_y)
	{
	}
};

int main(){

	std::vector<Vector2> vec;

	//適当な値のVector2を5個入れる
	for (int i = 0; i < 5; i++){
		vec.emplace_back(Vector2(rand() % 100, rand() % 100));
	}
	
	//出力
	for (auto iter = vec.begin(); iter != vec.end(); ++iter){
		std::cout << iter->x << " " << iter->y << std::endl;
	}

	return 0;
}
```

##vectorの要素の削除

erase関数で要素を削除できる。引数には削除する要素のイテレータを入れる。

```cpp
#include <iostream>
#include <vector>

int main(){

	std::vector<int> vec;//int型の動的配列
 
	// 10個の要素を追加していく
	for(int i = 0; i < 10; i++ ){
		vec.emplace_back(i);
	}
 
	//イテレータを用いて出力
	for(auto iter = vec.begin(); iter != vec.end(); ++iter){
		std::cout << *iter << " ";
	}
	std::cout << std::endl;
	
	
	std::cout << "現在のサイズ : " << vec.size() << std::endl;
	
	//4つめの要素の消去
	vec.erase(vec.begin() + 3);

	//イテレータを用いて出力
	for(auto iter =vec.begin(); iter !=vec.end(); ++iter){
		std::cout << *iter << " ";
	}
	std::cout << std::endl;
	
	std::cout << "現在のサイズ : " << vec.size() << std::endl;
	
	return 0;
}
```


##条件にあった要素の削除
条件にあった複数の要素を削除するには以下のようにする。わりとトリッキー。  
（削除用の関数、remove_ifは次回紹介する。）  
vectorの要素を削除すると削除した要素を指していたイテレータが迷子になってしまう。erase関数を使って削除をすると、返り値として削除した要素の次の要素へのイテレータが返ってくるので、それをループ用のイテレータに入れる。

```cpp
#include <iostream>
#include <vector>

int main(){

	std::vector<int> vec;//int型の動的配列
 
	// 10個の要素を追加していく
	for(int i = 0; i < 10; i++ ){
		vec.emplace_back(i);
	}
 
	//出力
	for(auto iter =vec.begin(); iter !=vec.end(); ++iter){
		std::cout << *iter << " ";
	}
	std::cout << std::endl;
	
	std::cout << "現在のサイズ : " << vec.size() << std::endl << std::endl;
	
	//要素の消去（偶数を消去）
	auto it = vec.begin();
	while(it != vec.end()){
		if(*it % 2 == 0){//偶数だったら
			std::cout << *it << "を消去" << std::endl;
			//eraseの返り値をitで受ける（返り値は削除したものの次につながっていた要素へのイテレータ）
			it = vec.erase(it);
		}else{
			it++;
		}
	}
	std::cout << std::endl;

	//出力
	for(auto iter =vec.begin(); iter !=vec.end(); ++iter){
		std::cout << *iter << " ";
	}
	std::cout << std::endl;
	
	std::cout << "現在のサイズ : " << vec.size() << std::endl << std::endl;
	
	return 0;
}
```

##演習問題(コンソール)

1. 以下のようなクラスを用意した。MyClassのvectorを作り、適当なa,bの値(0~100ぐらい)をもつデータを10個ほど格納し、イテレータを用いたfor文によって表示せよ。  

		#include <iostream>

		class MyClass{
		public:
			int a;
			int b;

			MyClass(int _a, int _b):
				a(_a),
				b(_b)
			{
			}
		};

1. aがbより小さいデータを削除し、再度表示せよ。  



##演習問題(Siv3D)
今回はvol_3のSiv3D演習問題の続きから作るか、新しくサンプルプロジェクトからプログラムを書くことをオススメする。いずれにせよ今回はプレイヤーはいなくていいし、敵がポインタ経由でプレイヤーの情報を受け取る必要はない。  

1. 以下のようなEnemyクラスを用意した。Enemyのvectorを作り、敵を複数出せ。forループは、イテレータを用いて書くこと。

	>Enemy.h

		#pragma once
		#include <Siv3D.hpp>

		class Enemy {
		public:
			Vec2 pos;
			Vec2 velocity;
			Enemy(const Vec2& _pos);
			void update();
			void draw();
		};

	>Enemy.cpp

		# include "Enemy.h"

		Enemy::Enemy(const Vec2& _pos):
			pos(_pos),
			velocity(RandomVec2(5.0))
		{
			// RandomVec2(double length)
			// 半径length(今回は5.0)の2次元ベクトルを返す
		}

		void Enemy::update() {
			pos += velocity;
		}

		void Enemy::draw() {
			Circle(pos, 30.0).draw(Color(255, 0, 0));
		}

1. 前回の課題で、y座標が480を超えた敵(下の方の画面外に出た敵)を削除するようにせよ。  
1. 画面外に出た敵を削除するようにせよ。  
ヒント：```Window::Width()```関数でウィンドウの幅を、```Window::Height()```関数でウィンドウのサイズを取得できる。ちなみに、デフォルトの画面サイズは640*480である。

