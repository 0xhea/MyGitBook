# String

* [String API](http://www.cplusplus.com/reference/string/string/)

## 1. 相关函数

### 1. find

* 返回值: string::npos
* size_t find (const string& str, size_t pos = 0) const;  //查找对象--string类对象
* size_t find (const char* s, size_t pos = 0) const; //查找对象--字符串
* size_t find (const char* s, size_t pos, size_t n) const;  //查找对象--字符串的前n个字符
* size_t find (char c, size_t pos = 0) const;  //查找对象--字符

### 2. substr

```
string substr（size_t pos = 0，size_t len = npos）const;
```

* str2 = str.substr(begin, len) 拷贝str的一部分。
* 从字符位置pos开始并跨越len字符（或直到字符串的结尾，以先到者为准）。

### 3. 判断函数

* 判断是否为数字 isdigit

* 判断是否为字母 isalpha

* 判断是否为空格 isspace

## 2. 反向字符串

1. reverse(s);
2. reverse(str.begin(), str.end());
3. string s(str.rbegin(),str.rend());



## 3. 逻辑

1. 判断两字符串组成字母是否相同，先排序

