
<!-- vim-markdown-toc GFM -->

* [多态](#多态)
		* [多态的内部结构](#多态的内部结构)
		* [纯虚函数(抽象类)](#纯虚函数抽象类)
		* [纯虚析构](#纯虚析构)

<!-- vim-markdown-toc -->
# 多态

多态按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态.  
  
$C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。$


> 实例如下
```
class Perjet {
	public:
		int a;
		int b;
		virtual void speek() {  //  虚函数 
			cout << "基类函数" << endl;
		}
};

class BasePerjet: public Perjet {
	public:
		void speek() { //重写虚函数
			cout << "子类函数" << endl; 
		}
};

void Speek(Perjet &p) {  //  必须有继承关系
	p.speek();
}

int main(int argc,char *argv[]) {
	BasePerjet p;
	Perjet b;
	
	Speek(p);
	Speek(b);
	return 0;
}
```
<font size=5><b>$Run\ \ \  results:$ </b></font>  
![20210222200608](https://i.loli.net/2021/02/22/NKcgIJHzxMLd3ak.png)
> 使用不同的类调用相同的函数，竟然得到了不同的结果，c++真奇妙


<font size=4 color=red><b>指针的大小在32位系统中占用四个字节，在64位系统系统中占用8个字节,这是固定的</b></font>  
> 你可能在想为什么说这个，等下就知道了

### 多态的内部结构

> 在类中使用 `virtual` 创建虚函数，其实是在类中创建一个`vfptr` 的虚函数(表) 指针


在虚函数表内记录虚函数的地址  

|类名::函数|
|-------|

<font size=4><b>For example:</b></font>  

```
#include<iostream>
using namespace std;

class Perjet {
	public: 
		virtual void max() {
		}
};


int main(int argc,char *argv[]) {
	cout << sizeof(Perjet) << endl;
	return 0;
}
```
输出的结果是 8 	字节

<font size=4><b>现在我们使用普通的函数</b></font>  
```
class Perjet {
	public: 
		void max() {
		}
};
```
> 计算的大小是 1

![20210224141237](https://i.loli.net/2021/02/24/kXC7Qea5JlsMxTp.png)

<b>当使用虚函数继承，子类重写虚函数，会覆盖掉父类的虚函数</b>
<font size=4><b>代码实例</b></font>  
```
#include<iostream>
using namespace std;

template<typename T> // 基类
class Perjet {
	public: 
		virtual T GetNum() {  // 虚函数
			return 0;
		}
};

template<typename T>  // 加法
class BasePerjet: public Perjet<T> {
	public: 
		T a,b;
		T GetNum() {
			return a+b;
		}
};

template<typename T>  // 减法
class sunBase: public Perjet<T> {
	public: 
		T a,b;
		T GetNum() {
			return a-b;
		}
};

template<typename T>
	void Max(Perjet<T> &p) {
		cout << p.GetNum() << endl;
	}


int main(int argc,char *argv[]) {
	BasePerjet<int> p;
	p.a = 10;
	p.b = 29;
	Max(p);
	
	sunBase<float> s;
	s.a = 100;
	s.b = 20;
	Max(s);
	
	return 0;
}

```


> c++中非常提倡使用多态设计程序架构
### 纯虚函数(抽象类)

创建纯虚函数

```
// 创建抽象类

template<typename T>
class Perjet {
	public: 
		virtual T max(T i) = 0; 
};

```
<font size=4><b>

1, 抽象类无法实例化对象     
2, 抽象类的子类必须重写父类  
3, 类中有一个纯虚函数就为抽象类  

</b></font>  

> 代码
```
#include<iostream>
using namespace std;


// 抽象类无法实例化对象
// 抽象类的子类必须重写父类

template<typename T>
class Perjet {
	public: 
		virtual T max(T i) = 0;  // 创建抽象类
};

template<typename T>
class BasePerje:public Perjet<T> {
	public: 
		T max(T i) {
			cout << " 抽象类" << i << " << " << endl;
			return i;
		}
};

template<typename T>
void test(Perjet<T> &p,int i = 0) {
		cout << p.max(i) << endl;
}


int main(int argc,char *argv[]) {
	BasePerje<int> p;
	p.max(10);
	return 0;
}
```
> 实例代码总是有义使用前面的内容，来巩固前面的知识,此次使用的是模板

### 纯虚析构

用我们之前学过的代码当碰到虚构函数的时候

![20210225113659](https://i.loli.net/2021/02/25/nQPgNziRld9xSJr.png)

$The results:$  
![20210225113822](https://i.loli.net/2021/02/25/RoYxpZsfFXdKDyA.png)  

oh，发现没有执行我的子类函数析构
这个时候我们需要使用虚构函数了

> 纯虚基类正确的方式
```
class Perjet {
	public:
		virtual void max() = 0;
		virtual ~Perjet() {
		}
};
```
> $The results$  
![20210225114043](https://i.loli.net/2021/02/25/kvRuf6NqQrw4mLC.png)

<font size=4><b>纯虚析构</b></font>  

<font size=4 color=red bgcolor=black><b>我们知道，带有纯虚函数的类为抽象类，不能被实例化，只能被子类继承，所以当我们设计一个基类为抽象类时，可以把析构函数声明为纯虚析构函数，这样基类就是抽象类了。</b></font>  

纯虚析构
```
#include<iostream>
#include <string>
using namespace std;

class Perjet {
	public:
		virtual void max() = 0;
		virtual ~Perjet() = 0;
};

Perjet::~Perjet() {
	cout << "这是纯虚析构 " << endl;
}

class BasePerjet: public Perjet {
	public: 
		void max() {
			 cout << "小猫在说话" << endl;
		}
		~BasePerjet() {
			cout << "小猫死了" << endl;
		}
};

void test() {
	
	Perjet * p = new BasePerjet;  
	p->max();
	delete p; 
}

int main(int argc,char *argv[]) {
	test();
}
```

<font size=4><b>
$The results$:
</b></font>  

![20210225123150](https://i.loli.net/2021/02/25/fHCx94bNe8urjKF.png)
> <font size=4><b>牢记纯虚析构和虚析构的区别</b></font>  

