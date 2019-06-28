# String

* [String API](http://www.cplusplus.com/reference/string/string/)

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



## 逻辑

1. 判断两字符串组成字母是否相同，先排序

