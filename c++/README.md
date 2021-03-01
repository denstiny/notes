
<!-- vim-markdown-toc GFM -->

* [迟早学习笔记](#迟早学习笔记)
	* [缩窄转换](#缩窄转换)
	* [指针](#指针)
	* [函数指针](#函数指针)
	* [变量的引用](#变量的引用)
	* [引用](#引用)
	* [默认参数](#默认参数)
	* [函数重载](#函数重载)
		* [函数的参数相同类型时](#函数的参数相同类型时)
		* [引用重载](#引用重载)
	* [函数模板](#函数模板)
		* [实例代码](#实例代码)
		* [重载模板](#重载模板)
		* [实例](#实例)
		* [判断模板类型](#判断模板类型)
			* [使用方法](#使用方法)
		* [显示具体化模板](#显示具体化模板)
			* [选择模板](#选择模板)
			* [新增关键字 `decltype`](#新增关键字-decltype)
	* [单独编译](#单独编译)
		* [头文件](#头文件)
			* [头文件的定义](#头文件的定义)
		* [作用域和链接](#作用域和链接)
			* [5种变量存储方式](#5种变量存储方式)
			* [变量的索引](#变量的索引)
				* [多文件索引](#多文件索引)
				* [说明符号和限制符号](#说明符号和限制符号)
	* [new 用法](#new-用法)
	* [内存模型和名称空间](#内存模型和名称空间)
		* [`using`  声明](#using--声明)
			* [注意事项](#注意事项)
			* [`using` 编译指令 和 编译声明比较](#using-编译指令-和-编译声明比较)
	* [c++ 内存分区模型](#c-内存分区模型)
			* [程序执行时](#程序执行时)
	* [类与对象](#类与对象)
		* [构造函数](#构造函数)
		* [类的访问权限](#类的访问权限)
	* [类的具体学习](#类的具体学习)

<!-- vim-markdown-toc -->
# 迟早学习笔记  
<font size=3>`This is my c notes`</font>   
## 缩窄转换  
* 缩窄转换  
	* 当我们使用`int` 类型转换成 `short`   
	`int` 是 4 个字节 `short` 是两个字节 当我们将`int`的类型变量转换成 `short` 那么这个操作就是缩窄转换
	```c++
		std::cout << sizeof(int) << sizeok(short) << std::endl;
	```
	* 在初始化列表时，是禁止缩窄转换的 例如:
	```c++
	long list[3] {1,2,3.5}; // 这样是进制的
	```
* c++ 其他字符串类型    
<u>编写程序时通常会面对一些不同的编码格式，如Unicode和multibytes。在有关字符串的处理时尤其重要，系统编程时通常会遇到很多这样的问题，例如把wchar*的字符串转换为char*的字符串，有时还需要把char*类型的字符串转换为wchar*类型。下面提供几种解决方案。</u>
	* wchar  
	```c++
	wchar_t *wc = L"hello world!"; 

	```
* c++ 结构体  
	* 结构体`struct` 一个不同类型的数据集合
	```c++ 
	// 定义一个 students 结构体
	struct Students
	{
		char name[20];
		unsigned int age;
		unsigned float results;
	}
	```
	结构体的内存是一种对齐的格式的 <br>
	char 类型的占一个字节，unsigned int 占4 字节 那么char在分配内存的时候也是分配的四个字节 <u>结构体中的内存会对齐最大的内存</u>
## 指针 
* 指针 
	* 指针与普通变量是一种截然不同的存储结构  
	普通变量是将值当做常量，编译器在编译的时候自动为其分配内存  
	指针将内存地址当作常量，将存储的值作为派生量，使用*访问的是指针指向的地址上面的值  
	```c++
	int p = 2;
	int *d;
	d = &p;
	std::cout << d << *d << std::endl;  // 输出指针的值和指针的值所存储的值
	```
* c++ 数组的替代品
	* 模板类`vector`<br>
	```c++
	//使用方法
	vector<int> vi(2); 		// 定义一个int 类对象
	vi[0] = 2;
	vi.insert(vi.end(),1);  // 在末尾末尾插入一个 1
	cout << vi[0] << endl;
	```
	* 模板类 `array`<br>
	`vector` 功能强大但是，效率比较低，`array` 拥有和数组相等的效率,和数组一样是固定边界,但是比数组更加安全  
	```c++
	array<int,2> a; // 定义一个int array数组边界为2
	a[0] = 1;
	a[1] = 2;
	cout << a << endl;
	```
* 逗号表达式
	* 使用逗号运算符是为了把几个表达式放在一起，整个逗号表达式的值为系列中的最后一个值
	```c++
	// 	表达式1，表达式2,表达式返回值
	var = (count=19,incr=10,count+1);
	// 在这里，首先把 count 赋值为 19，把 incr 赋值为 10，然后把 count 加 1，最后，把最右边表达式 count+1
	//的计算结果 20 赋给 var。上面表达式中的括号是必需的，因为逗号运算符的优先级低于赋值操作符。
	```
* #### cctype
	* #### `isalpha()` <font size=2>判断是否为字母</font>
	* #### `ispunct()` <font size=2>判断是否为数字</font>
* 三目运算符
	* ?:
	```c++
	std::cout << (2 > 3 ? 10 : 19) << std::endl;
		//   2 比3 小,所以为true 返回19
	```
## 函数指针
*  声明一个函数指针
	```c++
	double *(*pr)(int); // pr 为函数的地址
	const double *f1(const double ar[] ,int n);
	// 下面为实例
	#include<iostream>
	using namespace std;
	const double *f1(const double ar[] ,int n);
	const double *f2(const double [] ,int n);
	const double *f3(const double * ,int n);

	int main(int argc,char *argv[])
	{
		const double *(*p1)(const double *,int) = f1;
		const double av[3] {1,2,3};
		cout << (*p1)(av,3) << " : " << *(*p1)(av,3) << endl;
		auto pd = f2;
		cout << pd(av,3) << " : " << *pd(av,3) <<  endl;
		return 0;
	}

	const double *f1(const double ar[] ,int n)
	{
		return ar;
	}
	const double *f2(const double ar[] ,int n)
	{
		return ar+1;
	}
	const double *f3(const double *ar ,int n)
	{
		return ar+2;
	}


	```

* 内敛函数
	* 运行过程
	* ![demo](./src/nei.png)
	* 内敛函数编译时会把函数替换成函数的副本代码，当调用多次的时候，会生成多个副本造成内存负担，适当选择使用,即使它速度很快
	* `inline` 标识内敛函数
	```c
	inline double prins(double num)
	{
		std::cout << "hello wrold" << num << std::endl;
	   return 0;
	}
	// 像这段代码，如果在main函数中调用则编译完成之后实际上是这样的
	main
	{
		// prins(3);
		{
			std::cout << "hello world" << 3 << std::endl;
		}
		return 0;
	}
	```
## 变量的引用
* c++ 新增了一种复合类型--引用变量。引用就是为已经定义的变量的别名
	* 创建引用变量
	```c
	int main(int argc,char *argv[])
	{
		int res = 10;
		int & rodents =  res; // rodents 就是tes,只是换了一个名称 
		cout << rodents << endl;
		return 0;
	}
	```
	* 引用必须是在创建时候就初始化,有点类似于 `const`
	* 引用作为函数的参数<br>
	`下面的实例修改变量的参数`
	```c
	void max(int & a);
	void swap(int *a);

	int main(int argc,char *argv[])
	{
		int a = 1;
		int s = 1;
		max(a);
		swap(&s);
		cout << a << endl;
		// cout a = 3
		cout << s << endl;
		// cout s = 3
		return 0;
	}
	void max(int &  a)
	{
		a += 2;
	}
	void swap(int *a)
	{
		*a += 2;
	}
	```
## 引用	  
* 程序员使用对象的参数的主要原因有两个
	* 程序员可以修改调用函数中的对象参数
	* 通过传递引用而不是整个数据对象，可以提高计算机的运行速度  
	* 什么时候使用引用
		* 对于传递值而不做修改的函数
			* 如果数据对象很小，如内置的数据结构，则按值传递
			* 如果是数组，则使用指针，这是唯一的选择，并将指针生命为`const` 的指针。
			* 如果对象是较大的数据结构，则使用`const`指针或`const`引用，以提高程序的执行效率 。
			* 如果数据对象是类对象，则使用`const`引用，类设计的语义，常常要求使用引用，这是`c**`新增的特性主要原因,因此传递类对象使用引用
		* 对于修改函数中的数据
			* 如果函数对象是内置的数据类型,则使用指针。
			* 如果对象是数组则使用指针。
			* 如果对象是结构，则使用指针或引用。
			* 如果对象是类对象，则使用引用。

## 默认参数
<font size=5>**`对于带参数的函数,必须从右想左添加默认值。依旧是说，要为某个参数设置默认值，就必须为他右边的所有参数设置默认值`**  </font>

```c
int haro(int n,int s = 1,int j = 5);
使用的时候我们可以提供一个两个或者三个参数
haro(1);
haro(1,2);
haro(1,2,3);
```
<font size=4>**`实参按从做到右的顺序依次赋值给相应的形参,因此不能跳过任何一个参数`**  </font>
*下面的代码错误的*

```c
haro(1,,0); //   这是不允许的
```
## 函数重载
<font size=3>*函数的多态，允许你使用多个同名函数实现不同的功能*</font>  
<font size=2>c艹允许定义相同的函数，条件是他们的参数条目不同</font>  
```c
void print(int a,char b);    // use #1
void print(char a,char b);   // use #2
void print(double a,char b); // use #3
// 使用print函数时，c艹编辑器，将根据采取的相应的用法使用有相应特征标的原型:
print(1,'s');                // use #1
print('2','s');              // use #2
print(1.0,'s');              // use #3
```
### 函数的参数相同类型时
```
#include<iostream>
using namespace std;
void a(int a);
void a(int * a);
int main(int argc,char *argv[])
{
	int *a = new int;
	*a = 1;
	int b = 2;
	a(a);  // 在这里无论是使用指针还是变量都是一个错误,他不与任何一个原型匹配 
	a(b);
	return 0;
}
void a(int a) { 
	cout << "int" << endl; }
void a(int * a) { 
	cout << "int *" << endl; }
```
<font size=3>编译器在检查函数的特征标时，把类型引用和类型本身视为同一个特征标,匹配函数时并不区分`const`和非`const`的变量</font>
```c
void dribble(const char *bits);
void dribble(char *bits);
int main(int argc,char *argv[])
{
	char *str;
	const char *st;
	dribble(st);  // use dribble(const char *)
	dribble(str); 	// use dribble(char *) 
	return 0;
}

void dribble(const char *bits) { cout << "cont char" << endl; }
void dribble(char *bits) { cout << "char" << endl; }

```
<font size=2 color=red>请记住是特征码而不是函数类型使得可以对函数重载</font>  
```c
int doue(int a);
double doue(int a);
// 这时错误的，c艹不允许这样的重载，返回值可以不同，但是特征标也必须不同
```
### 引用重载
```c
void sink(double & r1);
void sink(const double & r1);
void sink(double && r1);
```
<font size=2>**左值引用参数r1与可修改的左值`double`匹配;`const` 左值引用参数，与可修修改的的左值应用参数，`const`左值参数和右值参数（如两个参数和）匹配；最后，左值引用参数与左值匹配。注意到第一个和第三个的参数都匹配第二个。此时，将调用最匹配的版本**</font>  

## 函数模板
```c
template<typename name>  //创建模板类型
```
### 实例代码
<font size=1><b>程序在运行的时候,会检测变量类型，并创建相应的模板，如果为`int`类型，则创建一个`int`类型的模板函数,程序员看不到这些操作，是由编译器完成的</b></font>  
```c
template<typename Anytype>
void Swap(Anytype &a,Anytype &b);
int main(int argc,char *argv[])
{
	int i = 10;
	int b = 1;
	char font = 'c';
	char str = 's'
	Swap(i, b); // int & ,int &
	Swap(font,str); // char & ,char &
	cout << "a -=>" << i << "b -=>" << b << endl;
	return 0;
}

template<typename Anytype>
void Swap(Anytype &a,Anytype &b)
{
	Anytype temp;
	temp = a;
	a = b;
	b = temp;
}

```
<font size=1 color=red><b>如果需要将多个不同类型应用在不同的变量，则可以使用模板</b></font>  
### 重载模板
`模板和普通函数一样可以使用重载`
### 实例
```c
#include<iostream>
using namespace std;
template <typename T>
T Swap(T &a,T &d);
template <typename T>
T Swap(T &a,T &d,T & n);
int main(int argc,char *argv[])
{
	 int n = 10, s = 1;
	 char cs = 's',cn = 'n';
	 
	 cout << cn << cs << Swap(cn,cs) << endl;
	 
	 cout << cn << cs << Swap(cn,cs,cs) << endl;
	return 0;
}

template <typename T>
T Swap(T &a,T &d)
{
	 T ss;
	 ss = d;
	 d = a;
	 a = ss;
	 return a;

}
template <typename T>
T Swap(T &a,T &d,T & n)
{
	 T ss;
	 ss = d;
	 d = a;
	 a = ss;
	 return n;
}
```
### 判断模板类型
在c艹11中，新增了一个`type_traits` 库，可以判断函数类型
#### 使用方法
```c
is_same<T, int>::value  //  如果T 为int 则返回true 否则返回false
```
也可使用`typeid` 
```
int Swap(T &t)
{
	 if(typeid(T) == typeid(int))
			return 1;
	 return 2;
}
```
### 显示具体化模板
当我们使用`template`创建一个模板,如下:
```
template<typename t>
void Swap(t &a,t &b);

```
<b>当我们想设置特定的函数模板,比如当我们要为`t` 为int类型时，使用定制的模板我们可以这么做 ：</b>  
```
template <> void Swap<int>(int &a,int &b);

```
<font size=3><b>这样就为刚刚的模板创建了一个具体化的模板</b></font>  <br><br>
<font size=1><b>编译器在选择原型的时候偶，非模板类型优先于显示具体化和模板版本，而显示具体化优先于使用模板生成的版本</b></font>  
> 实例代码
```c
#include<iostream>
#include <type_traits>
using namespace std;

void Swap(float &a,float &b);
template<typename t>

void Swap(t &a,t &b);

struct job
{
	 int a;
};

template <> void Swap<int>(int &a,int &b);


int main(int argc,char *argv[])
{
	 
	 float a = 1;
	 float b = 2;
	 Swap(a, b);
	return 0;
}

template < > void Swap<int>(int &a,int &b)
{
	 cout << "a -=> " << a << " | " << "b -=> " << b << endl;
}

template<typename t> void Swap(t &a,t &b)
{
	 cout << "template typename" << endl;
}

void Swap(float& a,float &b)
{
	 cout << "Void Swap" << endl;
}

```
> 运行截图
![20210206152552](https://i.loli.net/2021/02/06/aQI7xw5eKFpYNrl.png)
如果完全匹配的两个函数都是模板函数，则具体化模板优先.  
>
#### 选择模板
<b> 在程序中我们可以手动的选择使用模板</b>  
如下:
<details>
<summary>点击查看详情</summary>

```c
#include <functional>
#include<iostream>
using namespace std;

template <class T>
T leasser(T a,T b);

int leasser (int a ,int b);

int main(int argc,char *argv[])
{
	 int a = 10,b = 3;
	 double x  = 12.3,y = 5.2;
	 cout << leasser(a,b) << endl; // use int
	 cout << leasser<int>(x,y) << endl; // use class
	 cout << leasser<int>(a,b) << endl; // use class
	 cout << leasser(x,y) << endl; // use class
	 return 0;
}


template <class T>
T leasser(T a,T b)
{
	 cout << "class" << endl;
	 return a-b;
}

int leasser (int a ,int b)
{
	 cout 	 << "int " << endl;
	 return a+b;
}

```

</details>
<details>
<summary>运行结果</summary>

![20210208160926](https://i.loli.net/2021/02/08/gIneM82Zkl3SpqK.png)

</details>

#### 新增关键字 `decltype`
```c
int x;
decltype(x) a; // a type int， a 的类型为int
decltype(1+0.3) a;
```

## 单独编译
<font size=5><b>和c语言一样，c++ 也鼓励程序员将组建函数放在独立的文件夹中.</b></font> 
### 头文件
`在头文件中应包含以下内容`

<font size =4>
<b>

- 函数原型
- 使用#define 或 const 定义的符号常量
- 结构声明
- 类声明
- 模板声明
- 内敛声明

</b>
</font>

#### 头文件的定义

<font size=6>

<font size=3>

> config.h 
</font>  

```c
#ifndef CONFIG_H_
#define CONFIG_H_
struct poar
{
 double dou;
 int as;
};
void show_poly(poar day);
#endif

```
<font size=3>

> file.cpp
</font>
```c
#include <iostream>
#include <cmath>
using namespace std;
#include "con.h"
int main()
{
	 read();
	 return 0;
}
```
</font>

<font size=3>

> file1.cpp
</font>

<font size=6>

```c
#include <iostream>
#include <cmath>
using namespace std;
#include "con.h"
int main()
{
	 read();
	 return 0;
}

```
</font>

### 作用域和链接
在c++ 中默认的的函数声明中函数的参数和变量的存储性为自动，作用域为局部, 没有链接性

: 实例如下代码

```c
#include<iostream>
using namespace std;
void max(int *a);
int main(int argc,char *argv[])
{
	 int * a;
	 int *b;
	 cout << a << endl;
	 {
			a = new int;
			cout << a << endl;
			*a = 10;
	 }
	 cout << a << endl;
	 max(b);
	 cout << b << endl;
	 cout << *b << endl;
	 return 0;

}

void max(int *a)
{
	 a = new int;
	 *a = 10;
}


```

<details>
<summary>运行结果</summary>

![20210210164523](https://i.loli.net/2021/02/10/snkCgVIurEx1qM9.png)
</details>

`auto` 在c和以前的c++ 版本中 `auto` 含义域现在完全不同,在以前的版本中，他用于，显示的指出变量为自动存储
在c++ 11 中，他为自动类型推断

`register` 最初由c语言引入的 ,他建议编译器使用cpu寄存器来存储自动变量;  
`register int a;`
旨在提升从访问变量的速度

#### 5种变量存储方式
|存储描述|持续性 |作用域 |链接性 |如何声明 |
|:-:|:-:|:-:|:-:|:-:|
|自动 |自动 |代码块 |  无|在代码块中 |
|寄存器 |自动 |代码块 |无 |在代码块中，使用关键字register |
|静态，无链接性 |静态 |代码块 |无 |在代码块，使用static |
| 静态，内部链接性|静态 |文件 |内部 |不在热河函数内，使用关键字static|
| 静态，外部，链接性|静态| 文件| 内部|不再任何函数内


#### 变量的索引

当要调用非当前作用域的变量时可以使用`extern` 进行索引
```
#include<iostream>
#include "file.h"
using namespace std;
int as(int a,int b);
int main(int argc,char *argv[])
{
	 extern int s; // 索引变量s 
	 s = 20;
	 cout << s << endl;
	 return 0;
}

int as(int a,int b)
{
	 return a+b;
}

int s = 10;  
```
##### 多文件索引
> external.cpp
```
#include<iostream>
using namespace std;

double war = 0.3;

void update(double dt);
void local();

int main(int argc,char *argv[])
{
	 cout << "Globl " <<  war << " dd" << endl;

	 update(0.1);
	 cout << "Globl " << war << endl;

	 local();
	 cout << war << endl;
	 return 0;
}

```
> supportt.cpp
```
#include<iostream>
using namespace std;

extern double war;

void update(double dt)
{
	 extern double war;
	 war += dt;
	 cout << "Update " << war;
	 cout << "degress ";
}
void local()
{
	 double war = 0.8;
	 cout << "local " << war << "degress";
	 cout << "But global warming  =" << ::war ;
	 cout << "degress" ;
}

```
> 运行结果

![20210210221325](https://i.loli.net/2021/02/10/Mk2wUtih9EQg7oj.png)

##### 说明符号和限制符号

- `mutable` 
`const` 可以限制 变量定义之后无法再进行修改，而`mutable` 可以指出结构或者类为 `const` 之后，依然可以保持可以修改的状态 
<details>
<summary>点击查看</summary>

```c
struct data
{
	 char name[20];
	 mutable int ese;
};
int main(int argc,char *argv[])
{
	 const data temp {"asdfasdf",10};
	 temp.ese = 1;
	 cout << temp.name << temp.ese << endl;
	 return 0;
}

```
> 运行结果
![20210210223138](https://i.loli.net/2021/02/10/R4APHjlqTtsQXIW.png)

</details>

## new 用法
当`new`失败时将返回 `std::bad_allos`

1. new(temp) type
```c
int a[10] ;
int *b = new (a) int[4]; // 从a中分配5个内存给b
```
> 实例
```c
#include <cstdio>
#include <iostream>
#include <stdio.h>
using namespace std;

int main(int argc,char *argv[])
{
	 char buff[100];
	 char *p = new(buff) char[90];

	 for(int b = 0;b < 10;b++)
		 buff[b] = 's'; // 将buff前10个变量初始化为 s

	 printf("%p\n",buff);

	 printf("%p\n",p);

	 for(int b = 0;b < 100;b++)
			cout << *(p+b) << endl;  // 输出所有p的内存地址存储的的值
	 return 0;
}

```
<details>
<summary>运行结果</summary>

![20210210230532](https://i.loli.net/2021/02/10/gWYu3BhcXd5RAGb.png)

</details>

## 内存模型和名称空间
`
在c++ 中，名称可以是变量、函数、结构、枚举、类、以及类和结构的成员。当随着项目的增大，名称相互冲突性也将增加。
c++ 提供了名称空间工具，以便更好的管理控制名称的作用域
`  

`namespace` 创建名称空间
>实例

```
namespace Jack
{
int pail;
...
}

// 创建一个反而Jack名称空间
```

<font size=4><b>名称空间可以是全局的也可以是位于另一个名称空间中</b></font>  

```
namespace Jack
{
int pail;
namespace js
{
int temp;
}
...
}

//创建一个 Jack名称空间，并在其中创建一个 js名称空间
```

### `using`  声明


>  测试命名空间

`using` 声明将名称添加到局部变量的声明区域中

```
#include<iostream>
using namespace std;
namespace Jakc {
int todo;
}
int main(int argc,char *argv[])
{
	 //using Jakc::todo;
	 int todo; // 由于using 生命将名声添加到局部生命区域中，因此无法在这个局部声明，重复定义相同的fetch
	 todo = 10;
	 cout << todo << endl;
	 cout << &todo << endl;
	 cout << &Jakc::todo << endl;
	 return 0;
}


```
> 运行结果

<details>
<summary>点击查看详情</summary>

![20210211125319](https://i.loli.net/2021/02/11/4Px7faUOb5RpVs6.png)


```
观察到命令空间中的`todo` 地址与 局部变量`todo`地址不同

```

</details>

#### 注意事项

```
namespace Jack
{
 int a;
}
namespace jill
{
	 int a ;
}

Jack::a;
jill::a;
不是相同的标识符，标识不同的内存单元
```

> 实例

```
namespace Jack {
int line;
}

namespace Jock {
int line;
}
int main(int argc,char *argv[])
{
	 Jack::line = 10;
	 Jock::line = 20;

	 int pal = 30;
	 cout << Jack::line << endl
			<< Jock::line << endl
			<< pal << endl;
	 return 0;
}

```
<details>
<summary>点击查看详情</summary>

![20210211131808](https://i.loli.net/2021/02/11/siyxkYnOPIEWtUH.png)

</details>

#### `using` 编译指令 和 编译声明比较


## c++ 内存分区模型
#### 程序执行时
- 代码区：存放函数体的二进制代码，由操作系统进行管理
- 全局区：存放全局变量和静态变量以及常量
- 栈区：由编译器自动分配释放，存放函数的参数值，局部变量等
- 堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收  
<font size=3><b>不同区域存放数据的意义j</b></font>  
 - 不同区域存放的数据，赋予不同的生命周期，给予我们更灵活编程

## 类与对象
### 构造函数
 `与类同名的函数`
 ```
 class sl
 {
		public:
			sl() // 构造函数  在对象创建的时候自动调用
			{
				 cout << "hello world" << endl;

			}
			~sl() // 构造函数  在对象结束时候自动调用
			{
				 cout << "see you" << endl;
			}
 }
 ```


### 类的访问权限
|`类型` |权限|
|:-:|:-:|
|`public` |共有 |
|`private` |私有 |
| `protected`|保护 |


## [类的具体学习](./c++类.md)
