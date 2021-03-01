
<!-- vim-markdown-toc GFM -->

* [C++ 文件操作](#c-文件操作)
		* [二进制文件写入](#二进制文件写入)
		* [二进制文件读取](#二进制文件读取)

<!-- vim-markdown-toc -->
# C++ 文件操作

<font size=4><b>
		fstream 读写操作  
		ofstream 写操作  
		ifstream 都操作  
</b></font>  

使用

```
void ReadWrite() {                       
	ofstream outfile("test.txt",ios::out);   // 输入流
	ifstream readfile("test.txt",ios::in);   // 定义输出流
                                         
	if(!readfile.is_open()) {              
		cout << "open file error" << endl;     // 判断文件是否打开成功 一般判断一个就可以了
		return;                              
	}                                      
	                                       
	                                       
	outfile << "hello world" << endl;        // 写入字符
                                         
	                                       
	string buf;                            
	while(getline(readfile,buf)) {           // 读取字符
		cout << buf << endl;                 
	}                                      
	outfile.close() ;                      
	readfile.close();                      
}                                        

```
打开方式
|打开方式|描述|
|:-|:-:|
|ios::app|追加模式。所有写入都追加到文件末尾|
|ios::ate|文件打开后自动定位到文件末尾
|ios::in|打开文件用于读取
|ios::trunc|设置该文件已经存在，其内容将在打开文件之前被截断，把文件长度设置为0
您可以把以上两种或两种以上的模式结合使用。例如，如果您想要以写入模式打开文件，并希望截断文件，以防文件已存在，那么您可以使用下面的语法：
```
ofstream outfile;
outfile.open("file.dat", ios::out | ios::trunc );

```
> 同时写入和读取
```

void Fstream() {
	fstream os("tes.txt",ios::in | ios::out);
	
	if(!os.is_open()) {
		cout << "file open error" << endl;
		return;
	}
	
	os << "**************" << endl;
	os << "* 姓名: 李明 *" << endl;
	os << "* 年龄: 18   *" << endl;
	os << "**************" << endl;
	
	string buf;
	os.seekp(0);  // 设置文件内指针的位置，0为文件首 // 因为写入操作，指针在最后，所以需要加上这一步
	
	while(getline(os,buf)) {
		cout << buf << endl;
	}
		
	os.close();
}
```

```
// 读取文件的其他方式
string buf;
	while(getline(os,buf)) {
		cout << buf << endl;
	}

char buffer[100];
	while(!ifos.eof()) {
		ifos.getline(buffer,100);
		cout << buffer << endl;

string buf;
	while (os >> buf) {
	cout << buf << endl;
	}
```


<font size=4><b>关闭文件</b></font>  
```
void close();
```
### 二进制文件写入
```
#include <iostream>
#include <fstream>
#include <string.h>
#include <string>
using namespace std;

class Perjet {
	public:
		char buf[100];
		int bufsize;
};


int main(int argc,char *argv[]) {
	
	char buf[100] = "asdf";
	Perjet p = {"张三",19};

	//Perjet p;
	fstream os("tst",ios::binary | ios::out | ios::in);
	os.write((const char *)&p, sizeof(Perjet));
	
	//os.read((char *)&p, sizeof(Perjet));
	//cout << "字符 :" << p.buf << "字符长度: " << p.bufsize << endl;
	os.close();
	return 0;
}
```

### 二进制文件读取 

```
#include <iostream>
#include <fstream>
#include <string.h>
#include <string>
using namespace std;

class Perjet {
	public:
		char buf[100];
		int bufsize;
};


int main(int argc,char *argv[]) {
	
	Perjet p;
	fstream os("tst",ios::binary | ios::in);
	os.read((char *)&p, sizeof(Perjet));
	cout << "字符 :" << p.buf << "字符长度: " << p.bufsize << endl;
	os.close();
	return 0;
}
```
