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

#### 捕获

| 捕获列表             | 说明                                                         |
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

##### 可变 lambda （值拷贝才需要）

* 默认情况下，对于一个值被拷贝的变量，lambda 不会改变其值。如果我们希望能改变一个被捕获的变量的值，就必须在参数列表首加上关键字 **mutable** 。因此，**可变 lambda 不能省略参数列表**。

```c++
void fcn3()
{
	size_t v1 = 42;
	auto f = [v1] () mutable { return ++v1; };  // f 可以改变它所捕获的变量的值
	v1 = 0;
	auto j = f();  // j 为 43
}
```

* 一个引用捕获的变量是否（如往常一样）可以修改依赖于此引用指向的是一个 const 类型还是一个非 const 类型。不需要加上 mutable。

```c++
void fcn4()
{
	size_t v1 = 42;  // 非 const
	auto f2 = [&v1] { return ++v1; };
	v1 = 0;
	auto j = f2();  // j 为 1
}
```

#### 返回类型

如果一个 lambda 体包含 return 之外的任何语句，则编译器假定此 lambda 返回 void。此时，如果不希望返回 void，那么必须要指定返回类型。

```c++
transform(vi.begin(), vi.end(), vi.begin(),
			[](int i) -> int
			{ if (i < 0) return -i; else return i; });
```

### 参数绑定

##### 标准库 bind 函数 ` <functional>`

可以将 bind 函数看做一个通用的函数适配器，它接受一个可调用队形，生成一个新的可调用对象来“适应”原对象的参数列表。

```c++
auto newCallable = bind(callable, arg_list);
```

其中，newCallable 本身是一个可调用对象，arg_list 是一个逗号分隔的参数列表，对应给定的 callable 的参数。即，当我们调用 newCallable 时，newCallable 会调用 callable，并传递给它 arg_list 中的参数。

arg_list 中的参数可能包含形如 \_n 的名字，其中 n 是一个整数。这些参数是“占位符”，表示 newCallable 的参数，它们占据了传递给 newCallable 的参数的“位置”。数值 n 表示生成的可调用对象中参数的位置：\_1 为 newCallable 的第一个参数，\_2 为第二个参数，依次类推。

```c++
auto wc = find_if(words.begin(), words.end(),
					bind(check_size, placeholders::_1, sz));
```

bind 调用生成一个可调用对象，将 check_size 的第二个参数绑定到 sz 的值。只有一个占位符 \_1 表示 check_size 只接受单一参数。

##### 使用 placeholds 名字

名字 \_n 都定义在一个名为 placeholds 的命名空间中，而这个命名空间本身定义在 std 命名空间中。

```c++
using std::placeholders::_1;
using namespace std::placeholders;
```

##### bind 的参数

可以用 bind 绑定给定可调用对象中的参数或重新安排其顺序。

```c++
auto g = bind(f, a, b, _2, c, _1);  // f 是一个可调用对象，他有5个参数
```

##### 用 bind 重排参数顺序

```c++
sort(words.begin(), words.end(), isShorter);        // 按单词长度由短至长排序
sort(words,begin(), words.end(), bind(isShorter, _2, _1));  // 由长至短排序
```

##### 绑定引用参数  ref 函数

```c++
for_each(words.begin(), words.end(),
		[&os, c](const string &s) { os << s << c; });  // os和c都为局部变量
// 用bind函数
ostream &print(ostream &os, const string &s, char c)
{
	return os << s << c;
}
for_each(words.begin(), words.end(),
		bind(print, ref(os), _1, ' '));
```

**ref** 函数 `<functional>`：返回一个对象，包含给定的引用，此对象是可以拷贝的。标准库中还有一个 **cref** 函数，生成一个保存 const 引用的类。



## 再探迭代器

除了为每个容器定义了的迭代器之外，标准库在头文件 `<iterator>` 中还定义了额外几种迭代器。

* 插入迭代器（insert iterator）：这些迭代器被绑定到一个容器上，可用来向容器插入元素。
* 流迭代器（stream iterator）：这些迭代器被绑定到输入或输出流上，可用来遍历所关联的 IO 流。
* 反向迭代器（reverse iterator）：这些迭代器向后而不是向前移动。除了 forward_list 之外的标准库容器都有反向迭代器。
* 移动迭代器（move iterator）：这些专用的迭代器不是拷贝其中元素，而是移动他们。

### 插入迭代器

| 操作            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| it = t          | 在 it 指定的当前位置插入值 t。假定 c 是 it 绑定的容器，依赖于插入迭代器的不同类型，<br />此赋值会分别调用 c.push_back(t)、c.push_front(t) 或 <br />c.insert(t, p)，其中 p为传递给 inserter 的迭代器位置 |
| *it, ++it, it++ | 这些操作虽然存在，但不会对 it 做任何事情。每个操作都返回 it  |

插入器有三种类型，差异在于元素插入的位置：

* **back_inserter** 创建一个使用 push_back 的迭代器。（只有容器支持 push_back 时才能使用）
* **front_inserter** 创建一个使用 push_front 的迭代器。（只有容器支持 push_front 时才能使用）
* **inserter** 创建一个使用 insert 的迭代器。此函数接受第二个参数，这个参数是一个指向给定容器的迭代器。元素将被插入到给定迭代器所表示的元素**之前**。

```c++
copy(lst.cbegin(), lst.cend(), front_inserter(lst2));  // 逆序
copy(lst.cbegin(), lst.cend(), inserter(lst3, lst3.begin()));  // 与front_inserter不同，顺序
```

front_inserter 生成的迭代器会将插入的元素顺序颠倒过来，而 inserter 和 back_inserter 则不会。P358

### iostream 迭代器

istream_iterator 读取输入流，ostream_iterato，r 向一个输出流写数据。

#### istream_iterator 操作

待补充

#### ostream_iterator 操作

待补充

```c++
ostream_iterator<int> out_iter(cout, " ");
copy(vec.begin(), vec.end(), out_iter);  // 打印所有元素
```

### 反向迭代器

待补充



## 泛型算法结构

### 5类迭代器

| 迭代器类别     | 说明                                 |
| -------------- | ------------------------------------ |
| 输入迭代器     | 只读，不写；单遍扫描，只能递增       |
| 输出迭代器     | 只写，不读；单遍扫描，只能递增       |
| 前向迭代器     | 可读写；多遍扫描，只能递增           |
| 双向迭代器     | 可读写；多遍扫描，可递增递减         |
| 随机访问迭代器 | 可读写；多遍扫描，支持全部迭代器运算 |

待补充

### 算法形参模式

待补充

### 算法命名规范

待补充



## 特定容器算法

待补充



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

