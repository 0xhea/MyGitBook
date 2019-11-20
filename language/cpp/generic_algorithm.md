# 泛型算法

## 定制操作

### 向算法传递函数

1. Sort

```c++
sort(arr,arr+len,less<>(type));  // 升序
sort(arr,arr+len,greater<>(type));  // 降序
```

### lambda 表达式

可调用对象：对于一个对象或一个表达式，如果可以对其使用调用运算符（()），则称它为可调用的。

可调用对象有：函数、函数指针、重载了函数调用运算符的类和lambda表达式。

```c++
[capture list](parameter list) -> return type { function body }
```

* 与任何函数类似，一个 lambda 具有一个返回类型、一个参数列表和一个函数体。但与函数不同，lambda 可能定义在函数内部。
* capture list（捕获列表）是一个 lambda 所在函数中定义的局部标量的列表（通常为空）
* return type、parameter list 和 function body 与任何普通函数一样，分别表示返回类型、参数列表和函数体。但是，lambda 必须使用尾置返回来指定返回类型。

```c++
auto f = [] { return 42; };
cout << f() << endl;  // 打印42
```

* 我们可以忽略参数列表和返回类型，但必须永远包含捕获列表和函数体。
* 在 lambda 中忽略括号和参数列表等价于指定一个空参数列表。
* 如果忽略返回类型，lambda 根据函数体中的代码推断出返回类型。如果函数体只是一个 return 语句，则返回类型从返回的表达式的类型推断而来。否则，返回类型为 void。

#### 向 lambda 传递参数

* 与普通函数调用类似，调用一个 lambda 时给定的实参被用来初始化 lambda 的形参。通常，实参和形参的类型必须匹配。
* 但与普通函数不同，lambda 不能有默认参数。因此，一个 lambda 调用的实参数目永远与形参数目相等。

```c++
stable_sort(words.begin(), words.end(),
			[](const string &a, const string &b)
				{ return a.size() < b.size(); });
```

#### 使用捕获列表

* 一个 lambda 通过将局部变量包含在其捕获列表中来指出将会使用这些变量。
* 可以在 [] 中提供一个以逗号分隔的名字列表，这些名字都是 lambda 表达式所在函数中定义的。

```c++
auto wc = find_if(word.begin(), word.end(),
	[sz](const string &a) { return a.size() >= sz; });

for_each(wc, word.end(),
	[](const string &s) { cout << s << endl; });  // 打印所有元素
```

* for_each 函数捕获列表为空却还是可以使用 cout，是因为我们只对 lambda 所在函数中定义的（非 static）变量使用捕获列表。一个 lambda 可以直接使用定义在当前函数之外的名字。
* 捕获列表只用于局部非 static 变量，lambda 可以直接使用局部 static 变量和他所在函数之外声明的名字。

#### 捕获方式

| 方式                 | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| []                   | 空捕获列表。lambda 不能使用所在函数中的变量。                |
| [names]              | names 是一个逗号分隔的名字列表，这些名字都是 lambda所在函数的局部变量。<br />默认情况下，捕获列表中的变量都被拷贝。名字前如果使用了&，则采用引用捕获方式。 |
| [&]                  | 隐式捕获列表，采用引用捕获方式。<br />lambda 体中所使用的来自所在函数的实体都采用引用方式使用。 |
| [=]                  | 隐式捕获列表，采用值捕获方式。<br />lambda 体将拷贝所使用的来自所在函数的实体的值。 |
| [&, identifier_list] | identifier_list 是一个逗号分隔的列表，包含0个或多个来自所在函数的变量。<br />这些变量采用值捕获方式，而任何隐式捕获的变量都采用引用捕获方式。<br />identiffier_list 中的名字前面不能使用 & |
| [=, identifier_list] | identifier_list 中的变量都采用引用捕获方式，而任何隐式捕获的变量都采用值捕获方式。<br />identiffier_list 中的名字不能包括 this，且这些名字之前必须使用 & |

##### 值捕获

* 与函数传值参数不同，被捕获的变量的值是在 lambda 创建时拷贝的，而不是调用时拷贝。

##### 引用捕获

* 当以引用捕获一个变量时，必须保证在 lambda 执行时变量是存在的。
* 如果可能的话，应该避免捕获指针或引用。

##### 隐式捕获

* 让编译器根据 lamba 体中的代码来推断我们要使用哪些变量。

* 当混合使用隐式捕获和显示捕获时，捕获列表中的第一个元素必须是一个 & 或 =。此符号指定了默认捕获方式为引用或值。
* 当混合使用隐式捕获和显示捕获时，显示捕获的变量必须使用与隐式捕获不同的方式。




## 再探迭代器

### 插入迭代器

#### back_inserter

当我们通过 back_inserter 迭代器赋值时，赋值运算符会调用 push_back 将一个具有给定值的元素添加到容器中。



***

**全局函数无法调用非静态成员函数**  [不能使用非静态成员函数说明](https://blog.csdn.net/lym940928/article/details/89353485)

* 可以使用bind调用非静态成员函数  [bind函数](https://ask.csdn.net/questions/259500)

## 1. 排序

### 1. Sort

* [791. 自定义字符串排序](https://leetcode-cn.com/problems/custom-sort-string/)  sort/lamda表达式

### 2. stable_sort

* [937. 重新排列日志文件](https://leetcode-cn.com/problems/reorder-log-files/)

### 3. 反向

* [832. 翻转图像](https://leetcode-cn.com/problems/flipping-an-image/)

* `reverse(begin(), end());`



**求两个数的最大公约数**

* `__gcd()`

