
<!-- vim-markdown-toc GFM -->

* [c++ 面向对象](#c-面向对象)
	* [运算符重载](#运算符重载)
		* [成员函数运算符重载](#成员函数运算符重载)
		* [全局函数运算符重载](#全局函数运算符重载)
		* [运算副重载函数发生重载](#运算副重载函数发生重载)

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


