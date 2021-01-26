# 迟早学习笔记
## c++ 学习笔记

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
