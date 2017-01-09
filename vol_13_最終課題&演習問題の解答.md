#C++Siv3D講座 最終課題＆演習問題解答

前半の課題をポリモーフィズムを用いて書き直せ。練習として、敵の基底クラスへのスマートポインタのvectorに派生クラスを入れ、敵の削除にはremove_ifとラムダ式を使うこと。解答例は例のごとく添付資料内にある。  

以下にVol10~Vol13の演習問題(コンソール)の解答例を示す。  

## vol_10課題(コンソール)
```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
	int a;

	MyClass(int _a) :
		a(_a)
	{
	}
};

int main() {
	std::vector<MyClass*> vec;

	// 適当な数でMyClassを追加
	for (int i = 0; i < 10; i++) {
		vec.emplace_back(new MyClass(rand() % 10));
	}

	// vectorの中身を表示
	for (const auto& i : vec) {
		std::cout << i->a << " ";
	}
	std::cout << std::endl;

	// 5以下の要素を削除
	auto it = vec.begin();
	while (it != vec.end()) {
		if ((*it)->a <= 5) {
			delete *it;
			it = vec.erase(it);
		}
		else {
			it++;
		}
	}

	// vectorの中身を表示
	for (const auto& i : vec) {
		std::cout << i->a << " ";
	}
	std::cout << std::endl;

	return 0;
}
```

## vol_11課題(コンソール)

```cpp
#include <iostream>
#include <vector>

class IAnimal {
public:
	double weight;//重さ

	IAnimal(int w) : weight(w) {

	}
	virtual ~IAnimal() = default;

	virtual void talk() {
		std::cout << "基底クラスのtalk関数が呼ばれました。 重さは:" << weight << std::endl;
	}

};

class Dog : public IAnimal {
public:
	Dog(int w) : IAnimal(w) {

	}

	~Dog() = default;

	void talk() override {
		std::cout << "わんわん　重さは:" << weight << std::endl;
	}
};

class Yojo : public IAnimal {
public:
	Yojo(int w) : IAnimal(w) {

	}

	~Yojo() = default;

	void talk() override {
		std::cout << "ふぇぇ…　重さは:" << weight << std::endl;
	}
};

class Cat : public IAnimal {
public:
	Cat(int w) : IAnimal(w) {

	}

	~Cat() = default;

	void talk() override {
		std::cout << "にゃー　重さは:" << weight << std::endl;
	}
};

int main() {
	std::vector<IAnimal*> animals;
	animals.emplace_back(new Dog(10));
	animals.emplace_back(new Yojo(20));
	animals.emplace_back(new Cat(30));
	
	for (const auto& animal : animals) {
		animal->talk();
	}

	return 0;
}
```


## Vol_12課題(コンソール)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <memory>

class Vector2 {
public:
	int x, y;
	Vector2(int _x, int _y)
		:x(_x),
		y(_y)
	{
	}
	~Vector2() {
		std::cout << "Vector2のデストラクタが呼ばれました(x, y) = " 
			<< "(" << x << "," << y << ") " << std::endl;
	}
};

int main() {
	std::vector<std::shared_ptr<Vector2>> vec;

	// 適当な数でMyClassを追加
	for (int i = 0; i < 10; i++) {
		vec.emplace_back(std::make_shared<Vector2>(rand() % 10, rand() % 10));
	}

	// vectorの中身を表示
	for (const auto& i : vec) {
		std::cout << "(" << i->x << "," << i->y << ") ";
	}
	std::cout << std::endl;

	
	// x > yの要素を削除
	auto rmvIter = std::remove_if(vec.begin(), vec.end(), [](const std::shared_ptr<Vector2>& a) {
		return a->x > a->y;
	});
	vec.erase(rmvIter, vec.end());

	// vectorの中身を表示
	for (const auto& i : vec) {
		std::cout << "(" << i->x << "," << i->y << ") ";
	}
	std::cout << std::endl;

	return 0;
}
```