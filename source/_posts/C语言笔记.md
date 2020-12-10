---

title: C语言笔记
tags:

  - C
  - code
  - base

categories: C
comments: false
---

学习自：C语言中文网

# 1 基础

## 1.1 字符编码

| 字符编码     | 说明                                       |
| ------------ | ------------------------------------------ |
| ASCII        | 只显示英文字符（基本拉丁字母）             |
| GB2312       | 简体中文字符集，1980年发布                 |
| GBK          | 中文字符集，在GB2312基础上扩展，1995年发布 |
| GB18030      | 中文字符集，GBK的再扩展，2000年发布        |
| Big5         | 繁体中文字符集，台湾、香港地区             |
| ISO/IEC 8859 | 欧洲字符集（ASCII的扩展）                  |
| Unicode      | 统一码，万国码                             |

<!-- more -->

| Unicode方案 | 说明                            |
| ----------- | ------------------------------- |
| UTF-8       | 变长编码方案，使用1~6个字节存储 |
| UTF-32      | 固定长度编码方案，始终4个字节   |
| UTF-16      | 使用2or4个字节存储              |

## 1.2 编译（Compile）、链接（Link）

编译器（Compiler）

编译：将程序转换成计算机能够识别的二进制文件

链接器（Linker）

链接：将目标文件(Object File)和系统组件（比如标准库、动态链接库等）结合起来。

## 1.3 数据类型

| 说明         | 数据类型 | 数据长度 |
| ------------ | -------- | -------- |
| 字符型       | char     | 1        |
| 短整型       | short    | 2        |
| 整型         | int      | 4        |
| 长整型       | long     | 4        |
| 单精度浮点型 | float    | 4        |
| 双精度浮点型 | double   | 8        |
| 无类型       | void     |          |

## 1.4 常用库

### 1.4.1 stdio.h

全称：standard input and output

```c
puts();//output string,自动换行
gets();//获取字符串

scanf();//获取输入
printf();//print format,格式化输出，换行须在后方加'/n' 

sizeof();//获取某个数据类型的长度

putchar(a);//输出单个字符
getchar();//获取一个字符

fflush(stdout);//清空缓冲区
```



### 1.4.2 wchar.h

```c
wchar_t a = L'A';  //英文字符（基本拉丁字符）
wchar_t b = L'9';  //英文数字（阿拉伯数字）
wchar_t c = L'中';  //中文汉字
wchar_t d = L'国';  //中文汉字
wchar_t e = L'。';  //中文标点
wchar_t f = L'ヅ';  //日文片假名
wchar_t g = L'♥';  //特殊符号
wchar_t h = L'༄';  //藏文
```



### 1.4.3 conio.h

```c
//只能用于windows平台
getche();//输入一个字符后立即读取，不用等待用户按下回车键，界面显示这个字符
getch();//立即读取输入的单个字符，不显示
```



### 1.4.4 string.h

```c
strcat(arrayName1,arrayName2);//string catenate,把两个字符串拼接在一起
strcpy(arrayName1,arrayName2);//字符串复制
strcmp(arrayName1,arrayName2);//字符串比较

```

# 2 指针

&：取地址

*：指向地址

*(&a)=a

&(*pa)=pa

| 定  义       | 含  义                                                       |
| ------------ | ------------------------------------------------------------ |
| int *p;      | p 可以指向 int 类型的数据，也可以指向类似 int arr[n] 的数组。 |
| int **p;     | p 为二级指针，指向 int * 类型的数据。                        |
| int *p[n];   | p 为指针数组。[ ] 的优先级高于 *，所以应该理解为 int *(p[n]); |
| int (*p)[n]; | p 为[二维数组](http://c.biancheng.net/c/array/)指针。        |
| int *p();    | p 是一个函数，它的返回值类型为 int *。                       |
| int (*p)();  | p 是一个函数指针，指向原型为 int func() 的函数。             |

# 3 结构体

```c
struct 结构体名{
    结构体所包含的变量或数组
};

enum week{ Mon = 1, Tues = 2, Wed = 3, Thurs = 4, Fri = 5, Sat = 6, Sun = 7 };

union 共用体名{
    成员列表
};
```

位域：

```c
struct bs{
    unsigned m;//4个字节Byte
    unsigned n: 4;//4bit
    unsigned char ch: 6;//6bit
};
```

# 4 知识点补充

```c
typedef oldName newName;

typedef char ARRAY20[20];
ARRAY20 a1, a2, s1, s2;//等价于char a1[20], a2[20], s1[20], s2[20];

/*
输出 max:20
*/
typedef int (*PTR_TO_FUNC)(int, int);
int max(int a, int b){
    return a>b ? a : b;
}
int main(){
    PTR_TO_FUNC pfunc = max;
    printf("max: %d\n", (*pfunc)(10, 20));
    return 0;
}
```

const:

```c
const int MaxNum=100;

const int *p1;//指针指向的数据只读
int const *p2;//同上
int * const p3;//指针自身的值不能被修改

//指针本身和它指向的数据都是只读
const int * const p4;
int const * const p5;
```

随机数：

```c
	srand((unsigned)time(NULL));//重新播种
    a = rand();//依据种子和正态分布产生随机数
```

# 5 文件操作

FILE是<stdio.h>头文件中的一个结构体，专门保存文件信息

## 5.1 fopen()

fopen()获取文件信息，包括文件名、文件状态、当前读写位置等，并将这些信息保存到一个FILE类型的结构体变量中，然后将该变量的地址返回。

```c
FILE *fp = fopen("demo.txt", "r");//只读
FILE *fp = fopen("D:\\demo.txt","rb+");//读和写
```



判断文件是否打开成功

> fopen()打开错误返回值是NULL

| 打开方式 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| "r"      | 以“只读”方式打开文件。只允许读取，不允许写入。文件必须存在，否则打开失败。 |
| "w"      | 以“写入”方式打开文件。如果文件不存在，那么创建一个新文件；如果文件存在，那么清空文件内容（相当于删除原文件，再创建一个新文件）。 |
| "a"      | 以“追加”方式打开文件。如果文件不存在，那么创建一个新文件；如果文件存在，那么将写入的数据追加到文件的末尾（文件原有的内容保留）。 |
| "r+"     | 以“读写”方式打开文件。既可以读取也可以写入，也就是随意更新文件。文件必须存在，否则打开失败。 |
| "w+"     | 以“写入/更新”方式打开文件，相当于`w`和`r+`叠加的效果。既可以读取也可以写入，也就是随意更新文件。如果文件不存在，那么创建一个新文件；如果文件存在，那么清空文件内容（相当于删除原文件，再创建一个新文件）。 |
| "a+"     | 以“追加/更新”方式打开文件，相当于a和r+叠加的效果。既可以读取也可以写入，也就是随意更新文件。如果文件不存在，那么创建一个新文件；如果文件存在，那么将写入的数据追加到文件的末尾（文件原有的内容保留）。 |

## 5.2 fclose()

```c
int fclose(FILE *fp);
```
## 5.3 字符形式读写文件
### fgetc()

file get char

```c
int fgetc(FILE *fp);
//fp为文件指针，fgetc()读取成功时返回读取到的字符，读取到文件末尾或读取失败时返回EOF(end of file)
```

### fputc()

file put char

```c
int fputc ( int ch, FILE *fp );
//ch为要写入的字符，fp为文件指针
```
## 5.4 字符串形式读写文件
### fgets()

file get string

```c
char *fgets(char *str,int n, FILE *fp);
//str为字符数组，n为要读取的字符数目，fp为文件指针
//遇见换行符结束读取
```

### fputs()

file put string

```c
int fputs(char *str, FILE *fp);
//str为要写入的字符串，fp为文件指针。写入成功返回非负数，失败返回EOF
```

## 5.5 数据块形式读写文件

fread()和fwrite()时应该以二进制形式打开文件

### fread()

```c
size_t fread(void *ptr, size_t size,size_t count, FILE *fp);
/*
ptr:内存区块的指针
size:表示每个数据块的字节数
count:表示要读写的数据块的块数
fp:表示文件指针
返回值:返回成功读写的块数，也即count
*/
    
```

对于 fread() 来说，可能读到了文件末尾，可能发生了错误，可以用 ferror() 或 feof() 检测

### fwrite()

```c
size_t fwrite ( void * ptr, size_t size, size_t count, FILE *fp );
```

对于 fwrite() 来说，肯定发生了写入错误，可以用 ferror() 函数检测。

## 5.6 格式化读写文件

fscanf()和fprintf()的读写对象不是键盘和显示器，而是磁盘文件

```c
int fscanf ( FILE *fp, char * format, ... );
int fprintf ( FILE *fp, char * format, ... );
```

## 5.7 随机读写文件

rewind()用来将位置指针移动到文件开头

```c
void rewind(FILE *fp);
```

fseek()用来将位置指针移动到任意位置

```c
int fseek(FILE *fp, long offset, int origin);
```

1. fp：文件指针
2. offset：偏移量，也就是要移动的字节数
3. origin：起始位置

## 5.8 ftell()

```c
long int ftell(FILE *fp);
//用来获取文件内部指针（位置指针）距离文件开头的字节数
```

# 6 内存分区

| 内存分区                 | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| 程序代码区 (code)        | 存放函数体的二进制代码。一个C语言程序由多个函数构成，C语言程序的执行就是函数之间的相互调用。 |
| 常量区 (constant)        | 存放一般的常量、字符串常量等。这块内存只有读取权限，没有写入权限，因此它们的值在程序运行期间不能改变。 |
| 全局数据区 (global data) | 存放全局变量、静态变量等。这块内存有读写权限，因此它们的值在程序运行期间可以任意改变。 |
| 堆区 (heap)              | 一般由程序员分配和释放，若程序员不释放，程序运行结束时由操作系统回收。[malloc()](http://c.biancheng.net/cpp/html/137.html)、[calloc()](http://c.biancheng.net/cpp/html/134.html)、[free()](http://c.biancheng.net/cpp/html/135.html) 等函数操作的就是这块内存，这也是本章要讲解的重点。  注意：这里所说的堆区与数据结构中的堆不是一个概念，堆区的分配方式倒是类似于链表。 |
| 动态链接库               | 用于在程序运行期间加载和卸载动态链接库。                     |
| 栈区 (stack)             | 存放函数的参数值、局部变量的值等，其操作方式类似于数据结构中的栈。 |

## 6.1 栈和堆的区别

栈区：系统分配和释放，不受程序员控制

堆区：完全由程序员掌控，自主可控

## 6.2 动态内存分配函数

### malloc()

原型：void* malloc(size_t size);

作用：在堆区分配 size 字节的内存空间。

返回值：成功返回分配的内存地址，失败则返回NULL。

注意：分配内存在动态存储区（堆区），手动分配，手动释放，申请时空间可能有也可能没有，需要自行判断，由于返回的是void*，建议手动强制类型转换。

###  calloc()

原型：void* calloc(size_t n, size_t size);

功能：在堆区分配 n*size 字节的连续空间。

返回值：成功返回分配的内存地址，失败则返回NULL。

注意：calloc() 函数是对 malloc() 函数的简单封装，参数不同，使用时务必小心，第一参数是第二参数的单元个数，第二参数是单位的字节数。

###  realloc()

原型：void* realloc(void *ptr, size_t size);

功能：对 ptr 指向的内存重新分配 size 大小的空间，size 可比原来的大或者小，还可以不变（如果你无聊的话）。

返回值：成功返回更改后的内存地址，失败则返回NULL。

###  free()

原型：void free(void* ptr);

功能：释放由 malloc()、calloc()、realloc() 申请的内存空间。

# 7 Socket

1. Windows下socket程序依赖Winsock.dll或ws2_32.dll，必须提前加载。
2. Windows使用“文件句柄”概念，区分socket文件和普通文件；socket（）返回值为SOCKET类型（即句柄）
3. Windows下使用recv()/send()函数发送和接收
4. 关闭socket时，Windows使用closesocket()函数

## 7.1 socket()

```c
SOCKET socket(int af, int type, int protocol);
//创建套接字
//SOCK_STREAM:TCP SOCK_DGRAM:UDP
```

## 7.2 bind()

服务器端：将套接字与特定的IP地址和端口绑定起来

```c
int bind(SOCKET sock, const struct sockaddr *addr, int addrlen);  //Windows
/*
sock:socket文件描述符
addr:sockaddr结构体变量的指针
addrlen:addr变量的大小
*/
```

## 7.3 connect()

客户端：用connect()函数建立连接

```c
int connect(SOCKET sock, const struct sockaddr *serv_addr, int addrlen);  //Windows

```

## 7.4 listen()

让套接字进入被动监听状态

```c
int listen(SOCKET sock, int backlog);
/*
sock:需要进入监听状态的套接字
backlog:请求队列的最大长度
*/
```

## 7.5 accept()

套接字处于监听状态时，可以通过accept()函数接收客户端请求

```c
SOCKET accept(SOCKET sock, struct sockaddr *addr, int *addrlen);  //Windows
/*
sock:服务器端套接字
*/

```

## 7.6 数据接收和发送

```c
//发送
int send(SOCKET sock, const char *buf, int len, int flags);
/*
sock:要发送数据的套接字
buf:要发送的数据的缓冲区地址
len:要发送的数据的字节数
flags:发送数据时的选项
*/
int recv(SOCKET sock, char *buf, int len, int flags);
```

