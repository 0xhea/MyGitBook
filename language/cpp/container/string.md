# string

* [String API](http://www.cplusplus.com/reference/string/string/)



## 顺序容器通用操作额外的 string 操作

### 构造 string 的其他方法

| 操作                     | 说明（n、len2 和 pos2 都是无符号值）                         |
| ------------------------ | ------------------------------------------------------------ |
| string s(cp, n)          | s 是 cp 指向的数组中前 n 个字符的拷贝。此数组至少应该包含 n 个字符 |
| string s(s2, pos2)       | s 是 string s2 从下标 pos2 开始的字符的拷贝。若 pos2>s2.size()，构造函数的行为未定义 |
| string s(s2, pos2, len2) | s 是 string s2 从下标 pos2 开始 len2 个字符的拷贝。若 pos2>s2.size()，构造函数的行为未定义。不管len2的值是多少，构造函数至多拷贝 s2.size()-pos2 个字符 |
| s.substr(pos, len)       | 返回 string s 从 pos 开始 len 长度的拷贝。如果开始位置超过了 s 的大小，则抛出 out_of_range 异常 |

* 这些构造函数接受一个 string 或一个 const char* 参数，还接受（可选的）指定拷贝多少个字符的参数。

通常当我们从一个 const char* 创建 string 时，指针指向的数组必须以空字符结尾，拷贝操作遇到空字符时停止。如果我们还传递给构造函数一个计数值，数组就不必以空字符结尾，但计数值不能大于数组大小。

#### substr 操作

substr 操作返回一个 string，它是原始 string 的一部分或全部的拷贝。可以传递给 substr 一个可选的开始位置和计数值：

```c++
string s("hello world");
string s2 = s.substr(0, 5);   // s2 = hello
string s3 = s.substr(6);      // s3 = world
string s4 = s.substr(6, 11);  // s4 = world
string s5 = s.substr(12);     // 抛出 out_of_range 异常
```

### 改变 string 的其他方法

#### 额外的 insert 和erase 版本

除了接受迭代器的 insert 和erase 版本外，string 还提供了接受下标的版本。下标指出了开始删除的位置，或是 insert 到给定值之前的位置：

```c++
s.insert(s.size(), 5, '!');  // 在 s 末尾插入5个感叹号
s.erase(s.size() - 5, 5);    // 从 s 删除最后5个字符
```

还提供接受C风格字符数组的 insert 和 assign 版本。

```c++
const char *cp = "Stately, plump Buck";
s.assign(cp, 7);            // s == "Stately"
s.insert(s.size(), cp + 7);  // s == "Stately, plump Buck"
```

#### append 和 replace 函数

append 操作是在 string 末尾进行插入操作的一种简写形式。

replace 操作是调用 erase 和 insert 的一种简写形式。

```c++
s2.append(" world");        // 在字符串末尾插入
s2.replace(11, 3, "Fifth"); // 删除3个字符，并在其位置插入5个字符
```

append 和 replace 支持的参数格式很多，请参考 P323（待补充）。

### string 搜索操作

待补充

### compare 函数

待补充

### 数值转换

待补充



## 相关函数

### 1. 查找

* [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/submissions/)

#### find

* 返回值: string::npos
* size_t find (const string& str, size_t pos = 0) const;  //查找对象--string类对象
* size_t find (const char* s, size_t pos = 0) const; //查找对象--字符串
* size_t find (const char* s, size_t pos, size_t n) const;  //查找对象--字符串的前n个字符
* size_t find (char c, size_t pos = 0) const;  //查找对象--字符

#### find_first_of/find_first_not_of

* *查找str中出现最早的**一个**字符*
* `size_t find_first_of (const string& str, size_t pos = 0) const noexcept;`
* 第二个参数为搜索开始的下标（包括）

#### find_last_of/find_last_not_of

* *查找str中出现最晚的**一个**字符*
* `size_t find_last_of (const string& str, size_t pos = npos) const noexcept;`
* 第二个参数为搜索截止的下标（包括）

### 2. substr

```
string substr（size_t pos = 0，size_t len = npos）const;
```

* ***第二个参数为长度***

* str2 = str.substr(begin, len) 拷贝str的一部分。
* 从字符位置pos开始并跨越len字符（或直到字符串的结尾，以先到者为准）。

### 3. 判断函数

* 判断是否为数字 isdigit
* 判断是否为字母 isalpha
* 判断是否为空格 isspace

### 4. 反向字符串

1. reverse(s);
2. reverse(str.begin(), str.end());
3. string s(str.rbegin(),str.rend());

### 5. 与char \*转换

* [C++ char*，const char*，string的相互转换](https://www.cnblogs.com/wuyepeng/p/9729943.html)

1. string转const char*（c_str()）
```
const char* c_s = s.c_str();
```

2. const char*转string（直接赋值即可）
```
const char* c_s ="abc";
string s(c_s);
```


