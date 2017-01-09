#C++Siv3D講座 最終課題＆演習問題解答

前半の課題をポリモーフィズムを用いて書き直せ。練習として、敵の基底クラスへのスマートポインタのvectorに派生クラスを入れ、敵の削除にはremove_ifとラムダ式を使うこと。解答例は例のごとく添付資料内にある。  

以下にVol9~Vol12の演習問題(コンソール)の解答例を示す。  

## vol_10課題(コンソール)

```cpp
#include <iostream>
#include <vector>

class IAnimal {
public:
	double weight;//重さ

	IAnimal(int w) : weight(w) {

	}

	virtual void talk() {
		std::cout << "基底クラスのtalk関数が呼ばれました。 重さは:" << weight << std::endl;
	}
};

class Dog : public IAnimal {
public:
	Dog(int w) : IAnimal(w) {

	}

	void talk() override {
		std::cout << "わんわん　重さは:" << weight << std::endl;
	}
};

class Yojo : public IAnimal {
public:
	Yojo(int w) : IAnimal(w) {

	}

	void talk() override {
		std::cout << "ふぇぇ…　重さは:" << weight << std::endl;
	}
};

class Cat : public IAnimal {
public:
	Cat(int w) : IAnimal(w) {

	}
	
	void talk() override {
		std::cout << "にゃー　重さは:" << weight << std::endl;
	}
};

int main() {
	std::vector<IAnimal*> animals;
	animals.emplace_back(new Dog(10));
	animals.emplace_back(new Yojo(20));
	animals.emplace_back(new Cat(30));

	for (auto i = animals.begin(); i < animals.end(); i++) {
		(*i)->talk();
	}

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

	MyClass(int _a) :
		a(_a)
	{
	}
};


// aが5以下だとtrue, そうでない場合はfalseを返す叙述関数
bool isMin5(const MyClass& ins) {
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
	auto rmvIter = std::remove_if(vec.begin(), vec.end(), isMin5);

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

	MyClass(int _a) :
		a(_a)
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
