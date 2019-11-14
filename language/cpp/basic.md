# Basic

## 基本类型

### 指针

#### 指针的浅层和深层复制

*拷贝构造函数需要注意这个问题*

1. 浅层复制：拷贝指针（内存释放会使拷贝后的指针失效，并且内存可能会被多次释放）
2. 深层复制：拷贝指针所指向的内容

- 空指针为 **nullptr**

### 引用

指向指针的引用：int *&r = p; （从右向左阅读）



### 处理类型

#### 类型别名

```
typedef std::string::size_type pos;
using pos = std::string::size_type;
```

#### 获取类型

##### decltype

##### auto



## io类

io类属于不能被拷贝的类型，只能通过引用传递。并且，因为读取和写入的操作会改变流的内容，所以两个函数接受的都是普通引用，而非对常量的引用。

| 头文件   | 类型                                                         | 作用                                                   |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| iostream | istream, wistream<br />ostream, wostream<br />iostream, wiostream | 从流读取数据<br />向流写入数据<br />读写流             |
| fstream  | ifstream, wifstream<br />ofstream, wofstream<br />fstream, wfstream | 从文件读取数据<br />向文件写入数据<br />读写文件       |
| sstream  | istringstream, wistringstream<br />ostringstream, wostringstream<br />stringstream, wstringstream | 从string读取数据<br />向string写入数据<br />读写string |

为了支持使用宽字符的语言，标准库定义了一组类型和对象来操纵 `wchar_t` 类型的数据。宽字符版本的类型和函数的名字以一个 `w` 开始。例如，wcin、wcout。宽字符版本的类型和对象与其对应的普通char版本的类型定义在同一个头文件中。

##### cin

##### cout

##### cerr

输出警告和错误信息

##### clog

输出程序运行时的一般性信息



## 一些关键字

### const

#### constexpr

常量表达式是指值不会改变并且在编译过程就能得到计算结果的表达式。

##### constexpr指针

初始化必须是nullptr或者0，或者是存储于某个固定地址中的对象（定义于所有函数体之外的对象）。

constexpr int *q = nullptr;  // 说明q是指向整数的常量指针，与const不同，constexpr将定义的对象置为顶层const

### new

**使用new进行内存分配**

### using





