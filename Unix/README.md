
<!-- vim-markdown-toc GFM -->

* [Unix 高级环境$notes$](#unix-高级环境notes)
	* [**$Process\ environment$**](#process-environment)
		* [进程终止](#进程终止)
		* [**$exit \  function$**](#exit---function)
		* [进程环境](#进程环境)
		* [<font size=5 color=blue><b>内存空间分配</b></font>](#font-size5-colorblueb内存空间分配bfont)
		* [函数跳转](#函数跳转)

<!-- vim-markdown-toc -->

# Unix 高级环境$notes$

## **$Process\ environment$**

### 进程终止

有8种方式使进程终止,其中五种为正常退出.

* 正常退出
	+ **从 `main` 中退出**
	+ **调用`exit(1)`**
	+ **调用`_Exit`或`_exit`**
	+ **最后一个线程从其启动例成退出**
	+ **从最后一个线程调用`pthread_exit`**
* 异常退出
	+ **调用`abort `**
	+ **接到一个信号**
	+ **最后一个线程对取消请求做出响应**

### **$exit \  function$**
```C++
#include<stdlib.h> 
void exit(int status); // 退出时做相应清理工作,然后进入到内核
void _Exit(int status); // 直接进入到内核
#include<unstd.h>
void _exit(int status); // 直接进入到内核
```

> `exit` 总是执行一个标准`I/O`的清理关闭操作:对于所有打开的流调用`fclose`
函数.

> <font size=4 color=red>3个函数都是带一个整形参数,作为进程状态</font>  

main 函数返回一个整形值与用该值调用`exit`等价,于是在main函数中  
`exit(0);`  
等价  
`return 0;`


```C
#include<stdio.h>
#include<stdlib.h>
void first(void) {
	printf("This is first function exit\n");
}

void tow(void) {
	printf("This is tow function exit\n");
}

int main()
{
	printf("this is main \n");
	atexit(first);
	atexit(tow);
	return 0;
}

```
> 输出  
![20210412191212](https://i.loli.net/2021/04/12/A8zG3uhqwLSHMFJ.png)

### 进程环境
`每个程序都接收到一张环境表.`
```
int main(int argc,char *argv[],char **envp) {
// envp为全局变量环境表的地址
	return 0;
}
```

```c

#include<stdio.h>
int main( int argc, char *argv[] ) {
	extern char **environ;
	for(int i = 0;i <= 5;i++)
		printf("%s\n",environ[i]);
	return 0;
}

```
> out
![20210412194122](https://i.loli.net/2021/04/12/7UmaoYcu9jePpzl.png)

### <font size=5 color=blue><b>内存空间分配</b></font>  
- $function$
	+ `malloc ` 分配指定字节数的内存区.此内存区初始值不确定.
	+ `calloc ` 分配指定字节数的内存区,此内存区初始值为0
	+ `realloc` 增大或减小以前分配区长度.当增大时,可能需将以前分配区的内容移动到另一个更大的区域,以便在尾部添加存储区,而新增区域内的初始化不确定.

### 函数跳转

