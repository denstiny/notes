
<!-- vim-markdown-toc GFM -->

* [STL标准库](#stl标准库)
	* [> 关联式容器：二叉树树结构，各元素之间没有严格的物理上的顺序关系。](#-关联式容器二叉树树结构各元素之间没有严格的物理上的顺序关系)
	* [String 字符串](#string-字符串)
		* [默认构造函数](#默认构造函数)
		* [字符串查找](#字符串查找)
		* [字符串的替换](#字符串的替换)
		* [比较函数](#比较函数)
	* [vector 容器](#vector-容器)
		* [迭代器](#迭代器)
			* [完整实例](#完整实例)
		* [string 字符存取](#string-字符存取)
		* [string 插入和删除操作](#string-插入和删除操作)

<!-- vim-markdown-toc -->



# STL标准库
> STL容器就是将运用最广泛的一些数据实现出来
常用的数据结构：数组，俩表，树，栈，队列，集合，映射表等这些容器分为序列式容器和关联式容器两种：
序列式容器：强调值的排序，序列式容器中的每个元素都有固定的位置。
> 关联式容器：二叉树树结构，各元素之间没有严格的物理上的顺序关系。
---
> 算法：有限的步骤解决逻辑问题和数学上的问题叫做算法（Algorithms ) 
算法分为：质变算法和非质变算法
质变算法： 式指运行过程中会更改区间内的元素内容。例如：拷贝，替换，删除等待。
非质变算法：指运行过程中不会更改区间内的元素内容。例如：查找，计数，遍历，寻找极值等待。

迭代器：容器和算法的粘合剂
提供一种方法，使指能够依寻仿某个容器包含的各个元素，而又不暴露该容器的内部表示方法。
每个容器都有自己的专属的迭代器，
迭代器非常类似于指针，初学节点我们可以闲理解为迭代器为指针。
| 种类|功能 |支持运算 |
|:-:|:-:|:-:|
|输入迭代器 |对数据的只读访问 |只读，支持++、==、！= |
|输出迭代器 |对数据的只写访问 |只写、支持++ |
|向前迭代器 |读写操作。并能向前推进迭代器 |读写，支持++,==,!= |
|双向迭代器 |读写操作。并能向前和向后操作 |读写，支持++，-- |
|随机访问迭代器 |读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 |读写，支持++，--,[n],-n，<,>,<=,>= |

常用的容器中迭代器为双向迭代器，和随机访问迭代器

<br>
## String 字符串
> string c++ 新增的字符串对象

### 默认构造函数
```
string s1; // 默认构造
const char * str = "hello world";
string s2(str); 

cout << s2 << endl;
string s3(s2); // 拷贝构造

cout << s3 << endl;
string s4(10,'a'); // 初始化 10 个 a

cout << s4 << endl;
```
### 字符串查找


`find` or `rfind` 
```
string a = "asdf";

a.find("as"); // 没有找到返回 -1 找到返回as的位置 （从0开始计数） 顺序查找
a.rfind("as"); // 和find 效果一样，反向查找
```

###  字符串的替换

`replace`

```
string str = "asdffasdfas";
str.replace(str.find("d"),3,"111"); // 查找字符d的位置，并从d开始后的三个字符替换为111
```

### 比较函数
```
string str1 = "hello world";
string str2 = "hello";
string str3 = "hello";
if(str2.compare(str3) == 0) { 
	cout << "string 等于" << endl;  // 当str2 等于 str3 时 返回 0
}else if (str2.compare(str3) > 0) {
	cout << "str1 > str 2" << endl;  // 当str2 大于 str3 时返回大于0的数
}else{
	cout << "str1 < str2" << endl;  // 当 str2 小于 str3 时返回小于0的数
}
```


## vector 容器
`vector`
> 创建一个容器
```
vector<int> v;
```
> 向容器中插入数据

```
v.push_back(1);
v.push_back(2);
v.push_back(3);
v.push_back(4);
v.push_back(5);
v.push_back(6);
v.push_back(7);
v.push_back(8);
v.push_back(9);
v.push_back(10);
```
### 迭代器
> 创建迭代器
```
vector<int>::iterator itbegin = v.begin();
```
> 遍历迭代器
```
for(;itbegin != v.end() ;itbegin++)
	cout << *itbegin << endl;
```
> 使用算法库遍历迭代器
```
void Print(int v) {
	cout << v << endl;
}
int main() {
	for_each(v.begin(),v.end(),Print);
}

```


#### 完整实例
```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Perjet {
	public:
		int i ;
		char b;
		Perjet(int a,char b){
			i = a;
			this->b = b;
		}
};
template<typename T>
void showCPrint(T t) {
	cout << t.i << " " << t.b << endl;
}


int ma() {
	vector<Perjet> p;
	Perjet a(1,'2');
	Perjet b(3,'4');
	Perjet c(5,'5');
	p.push_back(a);
	p.push_back(b);
	p.push_back(c);
	
	for(vector<Perjet>::iterator s = p.begin(); s != p.end();s++) {
		cout << s->i << "   " << s->b << endl;
	}
	
	for_each(p.begin(), p.end(), showCPrint<Perjet>);
	
	return 0;
}
int showprint(int v) {
	cout << v << endl;
	return v;
}

int main(int argc,char *argv[]) {
	vector<int> list {1,2,3,4,5,6,7,8,9};
	for_each(list.begin(), list.end(), showprint) ;

	cout << "ma funcun" << endl;
	ma();
	return 0;
}

```

### string 字符存取

> 可以通 `at` $or$ `[]` 来进行单个字符的存取

> `[]` 方法

```
for(int i=0;i < str.size();i++) {
	cout << str[i] << endl;
}
```
> `at` 方法
```
for(int i = 0; i < str.size();i++) {
	cout << str.at(i) << " ";
}
```
> 单个字符的写入
```
char st[] = "world";
for(int i = 0; i < strlen(st);i++) {
	if(i > str.size()) {
		str +=  st[i];
	}else
		str.at(i) = st[i];
}
cout << str << endl;
```
> $Complete code$

```
int main(int argc,char *argv[]) {
	string str = "hello";
	// 单个字符的读取
	for(int i=0;i < str.size();i++) { 
		cout << str[i] << endl;
	}
	
	for(int i = 0; i < str.size();i++) {
		cout << str.at(i) << " ";
	}
	cout << endl;
	// 单个字符的写入
	char st[] = "world";
	for(int i = 0; i < strlen(st);i++) {
		if(i > str.size()) {
			str +=  st[i];
		}else
			str.at(i) = st[i];
	}
	cout << str << endl;
	return 0;
}
```
### string 插入和删除操作

> 插入
```
string str;
str.insert(0,"111");
```
> 删除
```
string str = "hello wrold";
str.erase(1,3);
```

