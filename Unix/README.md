
<!-- vim-markdown-toc GFM -->

* [Unix 高级环境$notes$](#unix-高级环境notes)
	* [**$Process\ environment$**](#process-environment)
		* [进程终止](#进程终止)
		* [**$exit \  function$**](#exit---function)
		* [进程环境](#进程环境)
		* [<font size=5 color=blue><b>内存空间分配</b></font>](#font-size5-colorblueb内存空间分配bfont)
		* [函数跳转](#函数跳转)
			* [函数执行顺序](#函数执行顺序)
			* [使用 `setjmp.h`](#使用-setjmph)
		* [信号](#信号)
			* [信号处理](#信号处理)

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

#### 函数执行顺序
> 函数正常的运行顺序
```cpp

static void b(void) {
	printf("%s():Begin.\n",__FUNCTION__);
	printf("%s():Call a().\n",__FUNCTION__);
}
static void a(void) {
	printf("%s():Begin.\n",__FUNCTION__);
	printf("%s():Call b().\n",__FUNCTION__);
	b();
	printf("%s():b() returned. \n",__FUNCTION__);
	printf("%s():End.\n",__FUNCTION__);
}
int main(int argc,char **argv) {
	printf("%s():Begin.\n",__FUNCTION__);
	printf("%s():Call a().\n",__FUNCTION__);
	a();
	printf("%s():a() returned. \n",__FUNCTION__);
	printf("%s():End.\n",__FUNCTION__);
	/*
		main -> a -|    return main --> main -> end
				   |  		    	|
				   | 		    	|
				   -> b -> end b --> end a
	*/
	return 0;
}
```
> `__FUNCTION__`当前函数
<details>
<summary>点击查看</summary>

![20210413164857](https://i.loli.net/2021/04/13/gvwbfQGBZHyPxJm.png)

</details>


#### 使用 `setjmp.h`

> 函数原型
```c
#include <setjmp.h>

int setjmp(jmp_buf env);
int sigsetjmp(sigjmp_buf env, int savesigs);

void longjmp(jmp_buf env, int val);
void siglongjmp(sigjmp_buf env, int val);
```
> 实例
```cpp

#include <cstdio>
#include <csetjmp>
#include <iostream>
#include <cstdlib>
/*
	setjmp 跳转 
*/

static jmp_buf env;  // 存储上下文
 void b(void) {
	printf("%s():Begin.\n",__FUNCTION__);
	printf("%s():Call a().\n",__FUNCTION__);
}

 void a(void) {
	printf("%s():Begin.\n",__FUNCTION__);
	printf("%s():Call b().\n",__FUNCTION__);
	b();
	longjmp(env,1);
	printf("%s():b() returned. \n",__FUNCTION__);
	printf("%s():End.\n",__FUNCTION__);
}

int main(int argc,char **argv) {
	if(setjmp(env))
		exit(1);
		
	printf("%s():Begin.\n",__FUNCTION__);
	printf("%s():Call a().\n",__FUNCTION__);
	a();
	printf("%s():a() returned. \n",__FUNCTION__);
	printf("%s():End.\n",__FUNCTION__);

	/*
		main -> a -|    return main --> main -> end
				   |  			     	|
				   | 			 	    |
				   -> b -> end b --> end a
	*/
	return 0;
}
```

<details>
<summary>点击查看</summary>

![20210413185103](https://i.loli.net/2021/04/13/87C3nbpKMQv5YzZ.png)

</details>

```txt
首次调用时 `setjmp` 为 0, 由`longjmp `返回,则返回 `env`
```
### 信号

`signal` 捕获信号

```
#include <algorithm>
#include <cstdio>
#include <iostream>
#include <unistd.h>
#include <cstdlib>
#include <csignal>
#include <sys/wait.h>
using namespace std;
void sig_init(int) {
	printf("\nexit %d\n ",getpid());
	exit(0);
}
int main(int argc,char **argv) {
	if(signal(SIGINT, sig_init) == SIG_ERR) {
		perror("signal error");
		exit(0);
	}
	while (1) {
		cout << "SIGNAL" << endl;
		sleep(1);;
	}
	return 0;
}
```
`signal(信号,处理)`

#### 信号处理
```C
SIG_IGN //忽略信号
SIGCHLD //子进程退出信号
```

```c

void sig_init(int sig) {
	printf("child to %d\n",sig);
	wait(0);
}


void out(int i) {
	for(int n = 0;n < i;n++) {
		cout << getpid() << " to " << n << endl;
		sleep(2);
	}
}

int main(int argc,char **argv) {
	if(signal(SIGCHLD, sig_init) == SIG_ERR) { // 处理子进程,子进程退出,则回首子进程
		perror("signal error");
		exit(0);
	}

	pid_t pid = fork();
	if(pid == 0) {
		out(10);
	}if(pid > 0) {
		out(100);
	}

	return 0;
}
```
