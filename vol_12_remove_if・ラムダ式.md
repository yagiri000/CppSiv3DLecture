#C++講座資料

vectorはコンテナの一種である。algorithmヘッダーにコンテナに対して削除・ソートなどを行う事ができる。
今回はコンテナに対し条件に一致する要素を削除する関数、remove_ifを使い、要素を削除する。また条件の指定にラムダ式を用いる。

## 要素を削除 std::remove_if

remove_ifは条件にあった要素を削除する。  
正確には、条件にあった要素を、vectorの後ろに配置しなおし、削除したように見せかけた後のvectorの最後のイテレータを返す。  つまり、remove_if関数を呼んだだけでは要素は削除されておらず、その後にerase関数を呼ぶ必要がある。  
remove_ifを使うには、algorithmヘッダーをインクルードする。

//remove_ifを実行し、条件にあった要素を後ろに詰める。
auto rmviter = std::remove_if(vec.begin(),vec.end(), 関数ポインタ);

//後ろの方に詰められた要素を削除
vec.erase( rmviter, vec.end() );


remove\_ifの第一引数には削除するかを判定する範囲の先頭、第二引数には終端を書く。
remove\_ifの第三引数には削除の条件を返す関数の名前を書く。（正確には関数ポインタ）

>3の倍数を削除する例

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

//3の倍数かどうかをたしかめて返す叙述関数
bool IsMultipleThree(int x){
	return (x%3) == 0;
}

int main(){

	std::vector<int> vec;//int型の動的配列

	//0から数を入れる
	for(int i = 0; i < 10; i++){
		vec.push_back(i);
	}

	//表示
	for(auto i = vec.begin(); i < vec.end(); ++i){
		std::cout << *i << " ";
	}
	std::cout << std::endl;


	//vecの中から3の倍数を後ろに詰める
	//3の倍数かどうかの判定は3番目のパラメータで渡された関数ポインタで判定
	auto rmviter = std::remove_if(vec.begin(), vec.end(), IsMultipleThree);

	//実際に削除
	vec.erase( rmviter, vec.end() );

	//表示
	for(auto i = vec.begin(); i < vec.end(); ++i){
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	return 0;
}
```

>remove\_ifの第三引数には、remove\_ifが対象にするvectorの中身を引数に取り、bool型(trueかfalse)を返す関数を入れる。以下が例。

```cpp
bool Func(配列の要素の型 x){
	return (x%3) == 0;//条件に一致してるか確認してbool型を返す
}
```

## 演習問題
1. 以下のようなクラスを定義した。MyClassのvectorを用意し、適当な値のa(0~10ぐらい)を持ったデータを10個格納し、aが5以下の要素をremove_ifを用いて削除せよ。


```cpp
class MyClass{
public:
	int a;

	MyClass(int aa):
		a(aa)
	{
	}
};
```



## ラムダ式
本項目ではC++の中でも特に書き方が変態的なラムダ式を扱う。
remove\_ifを使う時、大きなプログラムになると削除するかを判定する関数の定義と使う部分が離れてしまう。そこでC++11からはラムダ式というものが導入された。 例えば2で割り切れるかを返す関数オブジェクトを生成する場合  

```cpp
#include <iostream>

int main(){
    
    //2で割り切れるかどうかを返す関数オブジェクトを生成
    auto isMultiple2 = [](int num){return (num % 2) == 0;};
      
    std::cout << isMultiple2(3) << std::endl;
    std::cout << isMultiple2(4) << std::endl;
      
    return 0;
    
}
```


上のコードの

```cpp
[](int num){return (num % 2) == 0;}
```

の部分がラムダ式だ。 ラムダ式の型はautoで推論させている。(仕様上未定義なため)


ラムダ式の書き方

```cpp
[](引数){処理};
```

ところで返り値の型を指定していないことに気づいただろうか? これは多くの場合(ラムダ式内部でforを使うとかしない限り)指定しなくてよいものだ。
ラムダ式の返り値を明示的に指定する場合は

```cpp
[](引数)->返り値の型{
	処理
};
```

のように()のあとに「->(返り値の型)」という形式にする。


## ラムダ式とremove_if

ラムダ式を用いると、remove_if関数の近くに条件を判定する関数を書くことが出来る。

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main(){

	std::vector<int> vec;//int型の動的配列

	for(int i = 0; i < 10; i++){
		vec.push_back(i);
	}

	//表示
	for(auto i = vec.begin(); i < vec.end(); ++i){
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	
	//3の倍数かどうかを返す関数
	auto isMulThree = 
		[](int x){
			return (x % 3) == 0;
		};
	
	//vecの中から3の倍数を後ろに詰める
	auto rmviter = std::remove_if(vec.begin(), vec.end(), isMulThree);

	//実際に削除
	vec.erase( rmviter, vec.end() );


	//表示
	for(auto i = vec.begin(); i < vec.end(); ++i){
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	return 0;
}
```



remove_if関数の中に条件を判定する関数を直接書くことが出来る。

下の例では、第三引数に直接ラムダ式を書いている。

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main(){

	std::vector<int> vec;//int型の動的配列

	for(int i = 0; i < 10; i++){
		vec.push_back(i);
	}

	//表示
	for(auto i = vec.begin(); i < vec.end(); ++i){
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	
	
	
	//vecの中から3の倍数を後ろに詰める
	auto rmviter = std::remove_if(vec.begin(),vec.end(),
		[](int x){
			return (x % 3) == 0;
		}
	);

	//実際に削除
	vec.erase( rmviter, vec.end() );


	//表示
	for(auto i = vec.begin(); i < vec.end(); ++i){
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	return 0;
}
```

## 演習問題

1. int型の変数を引数にとり、2倍にして返す関数をラムダ式を用いて作成せよ。

1. int型のvectorに0から9を入れ、remove_ifとラムダ式を用いて2の倍数を削除せよ。

1. 以下のようなクラスMyClassを定義した。MyClassへのスマートポインタのvectorを用意し、適当な値のa(0~10ぐらい)を持ったデータを10個格納し、aが5以下の要素をremove_ifを用いて削除せよ。  
MyClassへのスマートポインタのvectorとは、`std:vector<std::shared_ptr<MyClass>> vec`である。  



```cpp
class MyClass{
public:
	int a;
	MyClass(int aa):
		a(aa)
	{
	}
};
```

## 演習問題(DXライブラリ)

1. Enemyのクラスへのスマートポインタを要素に持つvectorを用意し、敵を複数生成せよ。  
領域確保はnewではなく、std::make_sharedを使うこと。要素の削除にはalgorithmのremove\_ifを使い、その条件の指定にはラムダ式を使用せよ。  
