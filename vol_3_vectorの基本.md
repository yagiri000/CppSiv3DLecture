#C++講座資料

今回は動的配列vectorの基本について学ぶ。
演習では、DXライブラリを用いて複数の敵を生成する。


## vectorの基本的な使い方

C言語の配列は固定長だったが、C++には必要に応じて自動的にサイズを変更してくれる配列、vectorがある。
vectorの要素には配列と同じように[]でアクセス出来る。
vectorを使うときはvectorヘッダをインクルードする。(vector.hでないことに注意！)

```cpp
#include <iostream>
#include <vector>

int main(){

    std::vector<int> vec;//int型の動的配列
 
    // 10個の要素を追加していく
    for(int i = 0; i < 10; i++ ){
        vec.push_back(i);
    }
 
    //出力
    for(int i = 0; i < vec.size(); i++ ){
        std::cout << vec[i] << " ";
    }
    std::cout << std::endl;
    
    std::cout << "現在のサイズ : " << vec.size() << std::endl;
    
    return 0;
}
```

>書き方

```
std::vector<動的配列の中に入れる型> 動的配列の変数名;

例：std::vector<double> ary1;
```

>追加の仕方  
以下の例では、Vector2(x,y)でVector2の実体をつくり、push\_backによってvectorの末尾に追加している。

```
動的配列名.push_back(追加するデータ);

例：Vector2型の動的配列aryに Vector2(x,y)を入れるとしたら
ary.push_back(Vector2(x,y))
```

>配列のサイズ取得(返り値はunsigned int型)

```
動的配列名.size()
```

>クラスのvector例

```cpp
#include <iostream>
#include <vector>

class MyClass{
public:
	int x;

	MyClass(int xx){
		x = xx;
	}
};


int main(void){

	std::vector<MyClass> vec;

	//要素を追加
	for(int i = 0; i < 10; i++){
		vec.push_back(MyClass(rand()%10));
	}

	//要素MyClassのxを表示
	for(int i = 0; i < (int)vec.size(); i++){
		std::cout << vec[i].x << " ";
	}

	std::cout << std::endl;//改行

	return 0;
}
```

## 演習問題(コンソール)
1. int型のvectorを用意し、ランダムに0~9の数を10個入れて、すべて表示せよ。  
上記のvectorの中で5以下の要素を表示せよ。
>ヒント：ランダムな0~9の数を生成するにはrand()%10とすればよい。
（今回はsrandとかは呼ばなくていいです。より性能の高い乱数生成器が入っているrandomヘッダーに関して詳しく知りたければはこちら→[ああああ]）

1. 以下の様なクラスMyClassを用意した。MyClassのvectorを用意し、vectorにMyClassを10個追加し、すべての要素のShow()関数を呼び出せ。xの値はランダムな数でよい。

		class MyClass{
		public:
			int x;

			MyClass(int xx){
				x = xx;
			}

			void Show(){
				std::cout << "xは：" << x << std::endl;
			}
		};


1. 以下の様なVector2クラスを作った。  
Vector2クラスのvectorを用意し、rand()などを使って適当な座標を10個格納し、表示せよ。  

		class Vector2{
		public:
			int x;
			int y;
			
			Vector2(int xx, int yy){
				x = xx;
				y = yy;
			}
		};

## 演習問題(DXライブラリ)
今回は、前回のプロジェクトからそのままEnemyクラスを書き換えてもいいし、サンプルプロジェクトを元に1から作り直しても良い。

1. 以下の様なEnemyクラスを用意した。Enemyクラスのインスタンスを作り、動作を確認せよ。

	>Enemy.h

		class Enemy {
		public:
			double x, y, vx, vy;
			Enemy(double xx, double yy);
			void Update();
			void Draw();
		};

	>Enemy.cpp

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



1. Enemyのvectorを作り、Enemyが複数出られるようにせよ。

1. Zキーが押されたらランダムな座標に敵が出るようにせよ。

1. 現在、メインループ内でEnemyのvectorをfor文で回していると思う。  
メインループ内はできるだけすっきりさせたい。そこで、Enemyのvectorをメンバに持ち、Updateを呼ぶとメンバのEnemyのUpdateをすべて呼ぶEnemyManagerクラスを作り、メインループ内ではEnemyManagerのUpdateを呼ぶだけでEnemyすべてが更新され、EnemyManagerのDrawを呼ぶだけですべての敵が描画されるようにする。また、AddEnemy関数を持ち、それが呼ばれると敵を追加する。  
EnemyManagerクラスを作り、敵を管理せよ。  

		> EnemyManager.h

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

		> EnemyManager.cpp

			#include "EnemyManager.h"

			void EnemyManager::Update()
			{
				// ここを実装
			}

			void EnemyManager::Draw()
			{
				// ここを実装
			}

			void EnemyManager::AddEnemy(double x, double y)
			{
				// ここを実装
			}


1. Enemy.h, Enemy.cppとは別にEnemyManager.h, EnemyManager.cppを作り、適切にファイル分けをせよ。

1. 現在の敵の数を画面左上に表示せよ。ヒント：vectorのsize関数

