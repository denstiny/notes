
<!-- vim-markdown-toc GFM -->

* [c++ 面向对象](#c-面向对象)
	* [运算符重载](#运算符重载)
		* [成员函数运算符重载](#成员函数运算符重载)
		* [全局函数运算符重载](#全局函数运算符重载)
		* [运算副重载函数发生重载](#运算副重载函数发生重载)
	* [拷贝构造](#拷贝构造)
	* [友元](#友元)
	* [继承](#继承)
		* [继承方式](#继承方式)
		* [继承复用](#继承复用)
		* [继承的构建顺序](#继承的构建顺序)
		* [继承基类调用](#继承基类调用)
			* [静态变量](#静态变量)
			* [多重继承](#多重继承)
		* [虚继承](#虚继承)
	* [多态](#多态)

<!-- vim-markdown-toc -->

___

# c++ 面向对象

## 运算符重载
`operator` 创建一个重载运算符  

### 成员函数运算符重载

```
// 现在我们创建一个对象 
class Perjet {
	public: 
	// 定义两个int 类型变量
		int ba;
		int bb;

// 设置变量的值 
		Perjet() {}
		Perjet(int number,int number2) {
			ba = number;
			bb = number2;
		}

// 定义重载运算符
	Perjet operator+(Perjet &p) {  // 成员重载
		Perjet temp;
		temp.ba = this->ba + p.ba;
		temp.bb = this->bb + p.bb;
		return temp;
	}
};
```
调用
```
//  在调用的时候我们可以使用

	Perjet a(1,2);
	Perjet b(3,4);
	Perjet c = a + b;  // 重载 operator+  除了+还以使用其他的运算符
```

### 全局函数运算符重载
```
Perjet operator-(Perjet &p,Perjet &b) // 全局函数重载  
// 重载一个 - 号作用和成员函数重载一样
{
	Perjet temp;
	temp.ba = p.ba + b.ba;
	temp.bb = p.bb + b.bb;
	return temp;
}
```
调用 方法和成员函数一样
```
// 下面使调用这两个重载符号
cout << (a+b).ba << " | " << (a+b).bb << endl;
cout << (a-b).ba << " | " << (a-b).bb << endl;

```
<details>
<summary>输出</summary>

![20210217173727](https://i.loli.net/2021/02/17/VX7IfOCrUutxAGW.png)

</details>

### 运算副重载函数发生重载

运算符重载函数，是可以发生重载的，我们再定义一个+ 号的重载

```

Perjet operator+(Perjet &p,int num){
	Perjet temp;
	temp.ba = p.ba + num;
	temp.bb = p.bb + num;
	return temp;
}

cout << (a+10).ba << " | " << (a+2).bb << endl;

```
执行结果

![20210217175008](https://i.loli.net/2021/02/17/h5IHf4VZCFkE9xD.png)

## 拷贝构造
<font size=5><b>拷贝构造</b></font>  
在类中创建一个拷贝构造函数实例:

```
class Person
{
public:

  Person() {
    cout << "Person 构造函数" << endl;
  }

  Person(const Person &p) { // 拷贝构造
    // 拷贝构造
		cout << "拷贝构造" << endl;
    cout << p.age << endl;
    age = p.age;
  }

  void setAge(int a) {
    age = a;
  }
  void PAge()
  {
    cout << age << endl;
  }
private:
  int age;
};

```
<font size=3><b>使用</b></font>  
```
person p,b;
p(b);

```


## 友元

<font size=5><b>创建一个友元类</b></font>  
```
class pe;
class clxear {

	friend class pe;
	friend void max(clxear &p);  // 友元 是全局函数 max可以访问 私有变量和函数

	public:
	int a;
	private:
	int b;

};

```

## 继承

在使用 c++ 的时候因为要创建很多不同的类,但是这些不同的类可能存在同样地功能  
在此我们可以使用继承
首先我们创建一个类

```
class BaseHead { // 继承节点 配置
	public:
	void Print() {
		cout << "hello world!" << endl;
	}
};

```

使用继承

```
// 创建一个子类
class head:public BaseHead { // 子类使用父类 BaseHead 继承
// 子类也称为派生类
// 父类也称为基类
};

int main(int argc,char *argv[]) {
	head p;
	p.Print();
	return 0;
}

```

### 继承方式

![20210221104349](https://i.loli.net/2021/02/21/iu2gGv1VUtCrZkW.png)
案例
```
#include<iostream>
using namespace std;

class Perjet {
	public: // 公共继承
		void PuPrint() {
			cout << "This is my Public !" << endl;
		}
	private: // 私有继承
		void PrPrint() {
			cout << "This is my Private !" << endl;
		}
	protected: // 保护继承
		void PoPrint() {
			cout << "This is my Protected !" << endl;
		}
};


class OnePerjet: public Perjet  { // 共有继承
	public:
		OnePerjet() {
			PoPrint();
			PuPrint();
		}
};

class TowPerjet: private Perjet { // 私有继承
	public: 
		TowPerjet() {
			PoPrint();
			PuPrint();
		}
};

class TherePerjet: protected Perjet { // 保护继承
	public: 
		TherePerjet() {
			PoPrint();
			PuPrint();
		}
};
int main(int argc,char *argv[]) {
	
	class OnePerjet a;
	class TowPerjet b;
	class TherePerjet c;
	a.PuPrint();
	
	return 0;
}

```
<font size=5><b>注意基类中的私有内容，子类无法访问</b></font>  

![20210221112020](https://i.loli.net/2021/02/21/bmt1UXVCOgeQTjG.png)

### 继承复用
```
class BasePage: public OnePerjet { // 继承基类 BasePage
	public:
		BasePage() {
			cout << "BasePage" << endl;
			PuPrint();
			PoPrint();
		}
};
class BasePerje :private OnePerjet{
	public:
		BasePerje() {
			cout << "BasePerje" << endl;
			PuPrint(); 
			PoPrint();
		}
};


```

<font size=4><b>继承基类，会把基类所有的内容继承</b></font>  


```
#include<iostream>
using namespace std;

class BasePage {
	public:
		int a;
		BasePage() {
			cout << &b << endl;
		}
	private:
		int b;
	protected:
		int c;
};

class BaseOne:public BasePage {
	public:
		int ba;
		BaseOne() {
			cout << &b << endl;
			ba = 1;
			b = 2;
			cout << ba << " | " << b << endl;
		}
	private:
		int b;  
};

int main(int argc,char *argv[]) {
	BaseOne p;
	BasePage c;
	cout << sizeof(c) << endl;
	cout << sizeof(p) << endl;  // 基类中的所有内容都会继承   大小为20  编译器会自动帮你隐藏 私有内容

	return 0;
}

```

输出结果

![20210221121352](https://i.loli.net/2021/02/21/3gMB5KcpiALwIQ4.png)

### 继承的构建顺序

```

class Perjet {
	public:
		Perjet() {
			cout << "This is my Perjet" << endl;
		}
		~Perjet() {
			cout << "This is my Perjet" << endl;
		}
};

class BaseJet :public Perjet {
	public:
		BaseJet() {
			cout << "This is my BaseJet !" << endl;
		}
		~BaseJet() {
			cout << "This is my BaseJet !" << endl;
		}
};
int main(int argc,char *argv[]) {
	BaseJet p;
	// 继承先构建 基类，然后在构建子类，删除类的时候,先删除子类，然后删除基类
	return 0;
}
```


<font size=5 color=red><b>继承先构建 基类，然后在构建子类，删除类的时候,先删除子类，然后删除基类 </b></font>  

###  继承基类调用
当子类与父类拥有同名的成员变量 时，子类会覆盖父类继承的成员,直接调用会使用的是子类成员成员变量


```
class Perjet {
	public:
	int M_y;
	char M_S;
};

class BasePerjet:public Perjet {
	public:
		int M_S;
		BasePerjet() {
			M_y = 89;
			M_S = 89;
			Perjet::M_S = 'j';
		}
};

int main(int argc,char *argv[]) {
	BasePerjet a;
	cout << a.M_y  << " | " << a.M_S << endl;
	cout << a.Perjet::M_y  << " | " << a.Perjet::M_S << endl;
	return 0;
}

```
我们可以使用`a.Perjet::M_y` 来精准调用

> 运行结果

![20210221175930](https://i.loli.net/2021/02/21/M9t5WLyAZg2FmfD.png)

#### 静态变量
静态变量和普通变量使用一样
```

class Perjet {
	public:
		static int a;
};
int Perjet::a = 100; // 注意静态变量需要类外初始化

class BasePerje:public Perjet {
	public:
		static int a;
};
int BasePerje::a = 88;

int main(int argc,char *argv[]) {
	BasePerje p;
	
	cout << char(p.a) << " | " << char(p.Perjet::a)<< endl;
	return 0;
}

```

#### 多重继承
在类中使用 `class 子类 :继承方式 基类名称,继承方式 基类名称...`
使用案例
```
class Perjet {
	public:
		int a;
		static int max() {
			cout << "This is my Perjet" << endl;
			return 10;
		}
		Perjet () {
			a = 50;
		}
};

class BasePerjet {
	public:
		int a;
		static int max() {
			cout << "This is my BasePerjet" << endl;
			return 50;
		}
		BasePerjet() {
			a = Perjet::max();
		}
};

class BasePerjetOne :public Perjet,public BasePerjet {
	public:
		int a;
		static int max() {
			cout << "This is my BasePerjetOne" << endl;
			return 100;
		}
		BasePerjetOne() {
			a = BasePerjet::max();
		}
};

int main(int argc,char *argv[]) {
	BasePerjetOne p;
	
	p.max();
	p.BasePerjet::max();
	Perjet::max();
	
	cout << p.BasePerjet::a << endl;
	cout << p.Perjet::a << endl;
	cout << p.a << endl;
	return 0;
}
```
> 运行结果
![20210222135554](https://i.loli.net/2021/02/22/5H64tiGdPNcReW8.png)

> 使用调试工具查看内存

![20210222135625](https://i.loli.net/2021/02/22/JSh2TapykVwtQzW.png)


>> 可以看到 类`p` 继承了`BasePerjet` 和 `Perjet` 两个父类

### 虚继承
在前面的多继承中，发现了一个问题
就是当我们进行多继承的时候，会继承多份内容  
为了解决这个问题，我们可以使用虚继承

```
class Perjet {
	public: 
		int a;
		Perjet() {
			a = 10;
		}
};

class PerjetOne:virtual public Perjet {  //  virtual 虚继承的关键字
};

class PerjetTow:virtual public PerjetOne {
};


int main(int argc,char *argv[]) {
	PerjetTow p;
	cout << p.a << endl; 
	p.a = 100;  // 此时修改a的值
	cout << p.PerjetOne::a << endl;  // 输出基类的a
	//  输出结果为 100 ，可以发现虚继承类似使用共享内存
	return 0;
}
```

为了验证上面的猜想，我们打印一下地址
![20210222160106](https://i.loli.net/2021/02/22/NPtzbSR1Bc9Jip3.png)

发现地址是一模一样的，这正好验证了上面的猜想

## [多态](./polymorphism.md)
