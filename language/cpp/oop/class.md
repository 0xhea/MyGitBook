# class

定义在类内部的函数是隐式的inline函数。

使用class(private)和struct(public)定义类唯一的区别就是默认的访问权限。

## 定义抽象数据类型

### 构造函数

构造函数的任务是初始化类对象的数据成员，无论何时只要类的对象被创建，就会执行构造函数。

* 构造函数名字与类名相同

* 没有返回值
* 构造函数不能被声明成const的。构造函数在const对象的构造过程中可以向其写值，构造函数完成初始化过程后，对象才能真正取得其“常量”属性。

#### 默认构造函数

如果类没有显示地定义构造函数，那么编译器就会隐式地定义一个默认构造函数。编译器创建的构造函数又被称为**合成的默认构造函数**。

默认构造函数无需任何实参，会对没有初始值的成员执行默认初始化。

可以通过在参数列表后面写上**`= default`**使用合成的默认构造函数。`= default`既可以和声明一起出现在类的内部，也可以作为定义出现在类的外部。

使用默认构造函数进行初始化时，**不能在对象名后加括号**，否则会被识别成为函数定义。

```c++
Sales_data obj();  // 错误：声明了一个函数而非对象
Sales_data obj2;   // 正确
```

#### 构造函数初始值列表

```c++
Sales_data(const string &s) : bookNo(s) {}
Sales_data(const string &s, unsigned n, double p) : 
    bookNo(s), units_sold(n), revenue(p * n) {}
```

如果成员是const、引用，或者属于某种未提供默认构造函数的类类型，必须通过构造函数初始值列表为这些成员提供初始值。

成员的初始化顺序与他们在类定义中的出现顺序一致，构造函数初始值列表中的前后位置关系不会影响初始化顺序。

#### 委托构造函数

一个委托构造函数使用他所属类的其他构造函数执行它自己的初始化过程。

```c++
Sales_data(const string &s, unsigned n, double p) : 
	bookNo(s), units_sold(n), revenue(p * n) {}
Sales_data() : Sales_data("", 0, 0) {}
Sales_data(std::istream &is) : Sales_data() { read(is, *this) }
```

#### 隐式的类类型转换

如果构造函数只接受一个实参，则它实际上定义了转换为此类类型的隐式转换机制，这种构造函数成为**转换构造函数**。

```c++
Sales_data(const string &s) : bookNo(s) {}
Sales_data& combine(const Sales_data& s);
item.combine(string("9-999-999"));  // string隐式转换成Sales_data
```

只允许一步类类型转换。如果调用`item.combine("9-999-999")`是错误的，执行了两步转换，c_str需要先转换为string。

##### 抑制隐式类类型转换 explicit

```c++
explicit Sales_data(const string &s) : bookNo(s) {}  // 此构造函数不能用于隐式的创建Sales_data
```

关键字explicit只对一个实参的构造函数有效。只能在类内声明构造函数时使用explicit关键字，在类外部定义时不应重复。

#### 聚合类 （类似于c语言的struct）

* 所有成员都是public的
* 没有定义任何构造函数
* 没有类内初始值
* 没有基类，也没有virtual函数

```c++
struct Data {
	int ival;
	string s;
}
Data val1 = { 0, "Anna" };  // 初始化顺序必须和声明顺序一致，初始值列表中元素个数可以少于类的成员数量
```

#### 字面值常量类 P267

##### constexpr构造函数

一个字面值常量类必须至少提供一个constexpr构造函数。

***待理解，补充***

#### 移动构造函数

> 移动构造函数适用于：使用A对象初始化B之后，A对象就不需要再使用的情况。
> [C++移动构造函数以及move语句简单介绍](https://www.cnblogs.com/qingergege/p/7607089.html) 

##### move()函数:

- move函数会将指针使用后置为空值
- `vc.push_back(move(st));`  //执行后st.empty() = true

```c++
//移动构造函数
inline String::String(String &&other)  
{
    m_data = move(other.m_data);//变成右值
    other.m_data = NULL;//此处置空避免被析构
}
```

移动构造函数的写法：使用浅拷贝，然后将原对象置为空。

### 拷贝、赋值和析构

当类需要分配类对象之外的资源时，合成的版本常常会失效（例如分配动态内存）。

类中vector和string成员的拷贝、赋值和销毁的合成版本能够正常工作。

### 析构函数



## 访问控制和封装

* public: 说明符之后的成员在整个程序内可被访问，public成员定义类的接口。
* private: 说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码访问。

### 友元

在类定义的内部添加**`friend`**开头的函数声明。

* 友元函数可以定义在类内部，这样的函数是隐式内联的。

* 友元关系不存在传递性。（每个类负责控制自己的友元类或友元函数）

```c++
friend ostream &print(ostream &os, const Sales_data& item);  // 友元函数
friend class Window_mgr;  // 友元类
friend void Window_mgr::clear(ScreenIndex i);  // 友元成员函数，Window_mgr::clea必须在类之前被声明
```

类和非成员函数的声明不是必须在它们的友元声明之前。但是，如果想要使用友元，必须在友元声明后才行。

* **友元的声明仅仅指定了访问的权限，而非一个通常意义上的函数声明。**

```c++
struct X {  // P252
	friend void f() { /* 友元函数可以定义在类内部 */ }
	X() { f(); }             // 错误：f还没被声明
	void g();
	void h();
}
void X::g() { return f(); }  // 错误：f还没有被声明
void f();                    // 声明定义在X中的f()
void X::h() { return f(); }  // 正确：现在f的声明在作用域中了。
```



## 类的作用域

***返回类型中使用的名字位于类的作用域之外。***

用`Screen::height`的形式可以访问类的成员。用`::height`的形式可以访问外层作用域的变量。

### 名字查找

#### 用于类成员声明的名字查找

内层作用于可以重新定义外层作用域中的名字，即使该名字已经在内层作用域中使用过。**然而在类中，如果成员使用了外层作用域中的某个名字，而该名字代表一种类型，则类不能在之后重新定义改名字。**（类型名定义通常出现在类的开始处）



## 类的其他特性

### 类成员

#### 类型成员 typedef/using

用来定义类型的成员必须先定义后使用，这一点与普通成员有所区别。（类型成员通常出现在类开始的地方）

```c++
typedef std::string::size_type pos;
```

#### 内联函数 inline

定义在类内部的成员函数是自动inline的。

定义在类外部的成员函数**最好只在类外部定义的地方说明inline**。inline成员函数应该与相应的类定义在同一个头文件中。

#### 可变数据成员 mutable

如果希望在const成员函数内修改类的某个数据成员，在变量的声明中加入**`mutable`**关键字即可。

```c++
mutable size_t access_ctr;
```

#### 类数据成员的初始值

类内初始值 **必须使用符号=或者花括号** 的形式。

> [使用()初始化带来歧义](https://zhuanlan.zhihu.com/p/21102748)

```c++
class Window_mgr {
    std::vector<Screen> screens{Screen(24, 80, ' ')};
}
```

#### 类的静态成员

##### 声明静态成员

在成员之前加上关键字**`static`**使其与类关联在一起。静态成员可以是public的或private的。静态数据成员的类型可以是常量、引用、指针、类类型等。

**静态成员函数**也不与任何对象绑定在一起，它们不包含this指针。所以，静态成员函数不能声明成const的，也不能在static函数体内使用this指针。这一限制既适用于this的显式使用，也对调用非静态成员的隐式使用有效。

##### 使用类的静态成员

```c++
class Account {
public:
    void calculate() { amount += amount * interestRate; }  // 成员函数能直接使用静态成员
	static double rate() { return interestRate; }
private:
    double amount;
	static double interestRate;
    static double initRate();
}
double r;
r = Account::rate();  // 访问静态成员

Account ac1;
Account *ac2 = &ac1;
r = ac1.rate();       // 仍然可以使用类的对象、引用或者指针来访问静态成员
r = ac2-> rate();
```

##### 定义静态成员

即可以在类的内部也可以在类的外部定义静态成员函数。当在类的外部定义静态成员时，不能重复static关键字，该关键字只出现在类内部的生命语句。

静态数据成员不属于类的任何一个对象，所以它们并不是在创建类的对象时被定义的。这意味着它们不是由类的构造函数初始化的。而且一般来说，我们不能在类的内部初始化静态成员。相反的**必须在类的外部定义和初始化每个静态成员。一个静态数据成员只能定义一次。**

```c++
double Account::interestRate = initRate();  // 定义并初始化一个静态成员，从类名开始，这条定义语句的剩余部分就都位于类的作用域之内了。
```

##### 静态成员的类内初始化

通常情况下，类的静态成员不应该在类的内部初始化。然而，我们可以为静态成员提供**const整数类型的类内初始值，不过要求静态成员必须是字面值常量类型的constexpr。初始值必须是常量表达式。**

```c++
class Account {
public:
	static double rate() { return interestRate; }
	static void rate(double);
private:
	static constexpr int period = 30;  // period是常量表达式
	double daily_tbl[period];          // 使用常量表达式初始化数组
}
constexpr int Account::period;  // 初始值在类的内部提供，则成员的定义不能再指定初始值了。
```

即使一个常量静态数据成员在类内部被初始化了，通常情况下也应该在类的外部定义一下该成员。

##### 静态成员能用于某些场景，而普通成员不能

1. 静态数据成员可以是不完全类型（类在声明之后定义之前是不完全类型）。静态数据成员的类型可以就是它所属的类类型。而非静态数据成员则受到限制，只能声明成它所属类的指针或引用：

```c++
class Bar {
private:
	static Bar mem1;  // 静态成员可以是不完全类型
	Bar *mem2;        // 指针成员可以是不完全类型
}
```

2. 可以使用静态成员作为默认实参

```c++
class Screen {
public:
	Screen &clear(char = bkground);
private:
	static const char bkground;
}
```

### 返回*this的成员函数

一个const成员函数如果以引用的形式返回*this，那么他的返回类型将是常量引用。

#### 基于const的重载

```c++
Screen &display(std::ostream &os);
const Screen &display(std::ostream &os) const;
```

### 类类型

对于两个类来说，即使它们的成员完全一样，这两个类也是两个不同的类型。

对于一个类来说，在我们创建它的对象之前该类必须被定义过，而不能仅仅被声明。否则，编译器就无法了解这样的对象需要多少存储空间。类似的，类也必须首先被定义，然后才能用引用或者指针访问其成员（不知道有哪些成员）。

#### 类的声明

```c++
class Screen;
```

