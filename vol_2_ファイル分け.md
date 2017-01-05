#cpp講座資料

実際にゲームを作る時、ファイル分けをすることが必要になる。  
今回はクラスの内容を.hと.cppに分ける。


##ファイル分け
以下のように、.hと.cppにクラスの定義を分けて書くことが出来る。  
.cppには、クラス名::関数名と書く必要がある。以下は、コンソールでのプレイヤー例。  

>Player.h

```cpp
#include <iostream>

class Player{
public:	
	int x, y;

	Player(int xx, int yy);

	void Update();

	void Draw();

	void ShowXY();
};
```

>Player.cpp

```cpp
#include "Player.h"

Player::Player(int xx, int yy) :
x(xx),
y(yy)
{
}

void Player::Update(){
	std::cout << "PlayerのUpdateが呼ばれました。" << std::endl;
}

void Player::Draw(){
	std::cout << "PlayerのDrawが呼ばれました。" << std::endl;
}

void Player::ShowXY(){
	std::cout << "PlayerのShowXYが呼ばれました。 x:" << x << " y:" << y << std::endl;
}
```

>main.cpp

```cpp
#include <iostream>
#include "Player.h"

int main(void){

	Player ply(20, 30);

	ply.Update();

	ply.Draw();

	ply.ShowXY();

	return 0;
}
```


##演習問題(DXライブラリ)

1. 前回DXライブラリで作ったプロジェクト上で、Player.h, Plyaer.cpp, Enemy.h, Enemy.cppを作り、PlayerクラスとEnemyクラスをファイル分けせよ。
ヒント：Player.cppでは、keyStateを使うため、keyStateをインクルードする必要がある。  
