# object-oriented programming

## class

定义在类内部的函数是隐式的inline函数。

使用class(private)和struct(public)定义类唯一的区别就是默认的访问权限。

### 定义抽象数据类型

#### 构造函数

构造函数的任务是初始化类对象的数据成员，无论何时只要类的对象被创建，就会执行构造函数。

* 构造函数名字与类名相同

* 没有返回值
* 构造函数不能被声明成const的。构造函数在const对象的构造过程中可以向其写值，构造函数完成初始化过程后，对象才能真正取得其“常量”属性。

##### 默认构造函数

如果类没有显示地定义构造函数，那么编译器就会隐式地定义一个默认构造函数。编译器创建的构造函数又被称为**合成的默认构造函数**。

默认构造函数无需任何实参，会对没有初始值的成员执行默认初始化。

可以通过在参数列表后面写上**`= default`**使用合成的默认构造函数。`= default`既可以和声明一起出现在类的内部，也可以作为定义出现在类的外部。

使用默认构造函数进行初始化时，**不能在对象名后加括号**，否则会被识别成为函数定义。

```c++
Sales_data obj();  // 错误：声明了一个函数而非对象
Sales_data obj2;   // 正确
```

##### 构造函数初始值列表

```c++
Sales_data(const string &s) : bookNo(s) {}
Sales_data(const string &s, unsigned n, double p) : 
    bookNo(s), units_sold(n), revenue(p * n) {}
```

如果成员是const、引用，或者属于某种未提供默认构造函数的类类型，必须通过构造函数初始值列表为这些成员提供初始值。

成员的初始化顺序与他们在类定义中的出现顺序一致，构造函数初始值列表中的前后位置关系不会影响初始化顺序。

##### 委托构造函数

一个委托构造函数使用他所属类的其他构造函数执行它自己的初始化过程。

```
Sales_data(const string &s, unsigned n, double p) : 
	bookNo(s), units_sold(n), revenue(p * n) {}
Sales_data() : Sales_data("", 0, 0) {}
Sales_data(std::istream &is) : Sales_data() { read(is, *this) }
```



##### 移动构造函数

> 移动构造函数适用于：使用A对象初始化B之后，A对象就不需要再使用的情况。
> [C++移动构造函数以及move语句简单介绍](https://www.cnblogs.com/qingergege/p/7607089.html) 

###### move()函数:

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

#### 拷贝、赋值和析构

当类需要分配类对象之外的资源时，合成的版本常常会失效（例如分配动态内存）。

类中vector和string成员的拷贝、赋值和销毁的合成版本能够正常工作。

#### 析构函数

### 访问控制和封装

* public: 说明符之后的成员在整个程序内可被访问，public成员定义类的接口。
* private: 说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码访问。

#### 友元

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

### 类的作用域

***返回类型中使用的名字位于类的作用域之外。***

用`Screen::height`的形式可以访问类的成员。用`::height`的形式可以访问外层作用域的变量。

#### 名字查找

##### 用于类成员声明的名字查找

内层作用于可以重新定义外层作用域中的名字，即使该名字已经在内层作用域中使用过。**然而在类中，如果成员使用了外层作用域中的某个名字，而该名字代表一种类型，则类不能在之后重新定义改名字。**（类型名定义通常出现在类的开始处）

### 类的其他特性

#### 类成员

##### 类型成员 typedef/using

用来定义类型的成员必须先定义后使用，这一点与普通成员有所区别。（类型成员通常出现在类开始的地方）

```c++
typedef std::string::size_type pos;
```

##### 内联函数 inline

定义在类内部的成员函数是自动inline的。

定义在类外部的成员函数**最好只在类外部定义的地方说明inline**。inline成员函数应该与相应的类定义在同一个头文件中。

##### 可变数据成员 mutable

如果希望在const成员函数内修改类的某个数据成员，在变量的声明中加入**`mutable`**关键字即可。

```c++
mutable size_t access_ctr;
```

##### 类数据成员的初始值

类内初始值 **必须使用符号=或者花括号** 的形式。

> [使用()初始化带来歧义](https://zhuanlan.zhihu.com/p/21102748)

```c++
class Window_mgr {
    std::vector<Screen> screens{Screen(24, 80, ' ')};
}
```

#### 返回*this的成员函数

一个const成员函数如果以引用的形式返回*this，那么他的返回类型将是常量引用。

##### 基于const的重载

```c++
Screen &display(std::ostream &os);
const Screen &display(std::ostream &os) const;
```

#### 类类型

对于两个类来说，即使它们的成员完全一样，这两个类也是两个不同的类型。

对于一个类来说，在我们创建它的对象之前该类必须被定义过，而不能仅仅被声明。否则，编译器就无法了解这样的对象需要多少存储空间。类似的，类也必须首先被定义，然后才能用引用或者指针访问其成员（不知道有哪些成员）。

##### 类的声明

```c++
class Screen;
```

