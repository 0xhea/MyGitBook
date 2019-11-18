# 顺序容器

| 容器名       | 容器名                     | 特性                                             |
| ------------ | -------------------------- | ------------------------------------------------ |
| vector       | 可变大小数组               | 随机访问。在尾部之外的位置插入或删除元素很慢     |
| deque        | 双端队列                   | 随机访问。在头尾位置插入/删除速度很快            |
| list         | 双向链表                   | 双向顺序访问。插入/删除速度很快                  |
| forward_list | 单向链表                   | 单向顺序访问。插入/删除速度很快（无.size()成员） |
| array        | 固定大小数组（非内置数组） | 随机访问。不能添加或删除元素                     |
| string       | 字符串                     | 与vector类似                                     |

## 容器操作

| 容器操作               | 说明                                                       |
| ---------------------- | ---------------------------------------------------------- |
| **类型别名**           |                                                            |
| iterator               | 迭代器类型                                                 |
| const_iterator         | 只读的迭代器类型                                           |
| size_type              | 无符号整数类型                                             |
| difference_type        | 带符号整数类型，足够保存两个迭代器之间的距离               |
| value_type             | 元素类型                                                   |
| reference              | 元素的左值类型；与value_type&含义相同                      |
| const_reference        | 元素的const左值类型（const value_type&)                    |
| **构造函数**           |                                                            |
| C c;                   | 默认构造函数，构造空容器                                   |
| C c1(c2);              | 构造c2的拷贝c1                                             |
| C c(b, e);             | 将迭代器b和e指定的范围内的元素拷贝到c**（不适用于array）** |
| C c{a, b, c...};       | 列表初始化c                                                |
| **赋值与swap**         |                                                            |
| c1 = c2                | 将c1中的元素替换为c2中的元素                               |
| c1 = {a, b, c...}      | 将c1中的元素替换为列表中的元素                             |
| a.swap(b) / swap(a, b) | 交换a和b的元素                                             |
| **大小**               |                                                            |
| c.size()               | c中元素的数目**（不支持forward_list）**                    |
| c.max_size()           | c可保存的最大元素数目                                      |
| c.empty()              | 若c中存储了元素，返回false，否则返回true                   |
| **添加/删除元素**      | **不适用于array。在不同容器中，这些操作的接口都不同**      |
| c.insert(args)         | 将args中的元素拷贝进c                                      |
| c.emplace(inits)       | 使用inits构造c中的一个元素                                 |
| c.erase(args)          | 删除args指定的元素                                         |
| c.clear()              | 删除c中的所有元素，返回void                                |
| **关系运算符**         |                                                            |
| ==, !=                 | 所有容器都支持相等（不等）运算符                           |
| <, <=, >, >=           | 关系运算符（无序关联容器不支持）                           |
| **获取迭代器**         |                                                            |
| c.begin(), c.end()     | 返回指向c的首元素和尾元素之后位置的迭代器                  |
| c.cbegin(), c.cend()   | 返回const_interator                                        |
| **反向容器的额外成员** | **不支持forward_list**                                     |
| reverse_iterator       | 按逆序寻址元素的迭代器                                     |
| const_reverse_iterator | 不能修改元素的逆序迭代器                                   |
| c.rbegin(), c.rend()   | 返回指向c的尾元素和首元素之前位置的迭代器                  |
| c.crbegin(), c.crend() | 返回const_reverse_iterator                                 |

### 赋值和swap

| 操作                       | 作用                                                         |
| -------------------------- | ------------------------------------------------------------ |
| c1 = c2                    | 将c1中的元素替换为c2中元素的拷贝。c1和c2必须具有相同的类型   |
| c = { a, b, c... }         | 将c1中元素替换为初始化列表中元素的拷贝**（自己试验array可用）** |
| swap(c1, c2) / c1.swap(c2) | **交换c1和c2中的元素**。c1和c2必须具有相同的类型。swap(c1, c2)通常比c1.swap(c2)快得多 |
|                            | **assign操作不适用于关联容器和array**                        |
| seq.assign(b, e)           | 将seq中的元素替换为迭代器b和e范围中的元素。**b和e不能指向seq中的元素** |
| seq.assign(il)             | 将seq中的元素替换为初始化列表il中的元素                      |
| seq.assign(n, t)           | 将seq中的元素替换为n个值为t的元素                            |

#### assign

assign允许从一个不同但相容的类型赋值，或者从容器的一个子序列赋值。

```c++
list<string> names;
vector<const char*> oldstyle;
names = oldstyle;                                  // 错误：容器类型不匹配
names.assign(oldstyle.cbegin(), oldstyle.cend());  // 正确：const char*可以转换为string
```

#### swap 尽量使用非成员版本

除了array外，swap只是交换了两个容器的内部数据结构，元素本身并为交换。所以，除了string外，指向容器的迭代器、引用和指针不会失效，仍指向swap操作之前所指向的那些元素，也就是swap后指向了另外一个容器。（指向容器的引用和指针一直与交换后的容器保持一致，指向容器元素的迭代器、引用和指针仍指向原来的元素）

对一个string调用swap会导致迭代器、引用和指针失效。

swap两个array会真正交换它们的元素。指针、引用和迭代器不会失效，已经更换为swap后的值。

### 关系运算符

关系运算符左右两边的运算对象必须是相同类型的容器，且必须保存相同类型的元素。

只有当其元素类型也定义了相应的比较运算符时，我们才可以使用关系运算符来比较两个容器。容器的相等运算符实际上是使用元素的 == 运算符实现比较的，而其他关系运算符是使用元素的 < 运算符。



## 顺序容器操作

### 向顺序容器添加元素

* 这些操作会改变容器的大小；array 不支持这些操作。
* forward_list 有自己专有版本的 insert 和 emplace；参见下一小节“特殊的 forward_list 操作”。
* forward_list 不支持 push_back 和 emplace_back。
* vector 和 string 不支持 push_front 和 emplace_front。
* **向一个 vector、string 或 deque 插入元素会使所有指向容器的迭代器、引用或指针失效。**

| 操作                                    | 说明                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| c.push_back(t) / c.emplace_back(args)   | 在 c 的尾部创建一个值为 t 或由 args 创建的元素。返回 void    |
| c.push_front(t) / c.emplace_front(args) | 在 c 的头部创建一个值为 t 或由 args 创建的元素。返回 void    |
| c.insert(p, t) / c.emplace(p, args)     | 在迭代器 p 指向的元素**之前**创建一个值为 t 或由 args 创建的元素。返回指向新添加的元素的迭代器。 |
| c.insert(p, n, t)                       | 在迭代器 p 指向的元素之前插入 n 个值为 t 的元素。返回指向新添加的第一个元素的迭代器；若 n 为0，则返回 p |
| c.insert(p, b, e)                       | 将迭代器 b 和 e 指定的范围内的元素插入到迭代器 p 指向的元素之前。**b 和 e 不能指向 c 中的元素。**返回指向新添加的第一个元素的迭代器；若范围为空，则返回p |
| c.insert(p, il)                         | il是一个花括号包围的元素值列表。将这些给定值插入到迭代器 p 指向的元素之前。返回指向新添加的第一个元素的迭代器；若列表为空，则返回p |

#### 使用emplace操作

emplace_front、emplace 和 emplace_back，这些**操作构造**而不是拷贝元素。当调用 push 或 insert 成员函数时，我们将元素类型的对象传递给它们， 这些对象被拷贝到容器中。而当我们调用一个emplace成员函数时，则是将参数传递给元素类型的构造函数。emplace成员使用这些参数在容器管理的内存空间中直接构造元素。

传递给emplace函数的参数必须与元素类型的构造函数相匹配。

```c++
c.emplace_back("9-9999-999", 25, 15);
c.push_back(Sales_data("9-9999-999", 25, 15));
```

在调用 emplace_back 时，会在容器管理的内存空间中直接创建对象。而调用 push_back 则会创建一个局部临时对象，并将其压入容器中。

### 访问元素

* at 和下标操作只适用于 string、vector、deque 和 array。
* back 不适用于 forward_list。

| 操作      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| c.back()  | 返回 c 中尾元素的**引用**。若 c 为空，函数行为未定义         |
| c.front() | 返回 c 中首元素的**引用**。若 c 为空，函数行为未定义         |
| c[n]      | 返回 c 中下标为 n 的元素的**引用**，n 是一个无符号整数。若 n >= c.size()，则函数行为未定义 |
| c.at(n)   | 返回下标为 n 的元素的**引用**。如果下标越界，则抛出一 out_of_range 异常 |

* 如果容器是一个 const 对象，则返回值是 const 的引用。

#### at 与 [] 的区别

1. 在某些容器中 [] 可以用于添加元素，at则不行，比如：map
2. at 会检查下标的有效性，无效会抛出 std::out_of_range 异常，[] 则不会检查。*例外，在 array 中，两种操作都不会进行下标有效性检查*。

### 删除元素

* 这些操作会改变容器的大小，所以不适用于 array。
* forward_list 有特殊版本的 erase，参见下一小节“特殊的 forward_list 操作”。
* forward_list 不支持 pop_back；vector 和 string 不支持 pop_front。
* **删除 deque 中除首尾位置之外的任何元素都会使所有迭代器、引用和指针失效。指向 vector 或 string 中删除点之后位置的迭代器、引用和指针都会失效。**

| 操作          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| c.pop_back()  | 删除 c 中尾元素。若 c 为空，则函数行为未定义。函数返回 void  |
| c.pop_front() | 删除 c 中首元素。若 c 为空，则函数行为未定义。函数返回 void  |
| c.erase(p)    | 删除迭代器 p 所指定的元素，返回一个**指向被删元素之后元素的迭代器**，若 p 指向尾元素，则返回尾后（off-the-end）迭代器。若 p 是尾后迭代器，则函数行为未定义 |
| c.erase(b, e) | 删除迭代器 b 和 e 所指定范围内的元素。返回一个指向最后一个被删元素之后元素的迭代器，若 e 本身就是尾后迭代器，则函数也返回尾后迭代器。 |
| c.clear()     | 删除 c 中的所有元素。返回 void                               |

### 特殊的 forward_list 操作

* 由于 forward_list 是单向链表。在一个 forward_list 中添加或删除元素的操作是通过改变给定元素之后的元素来完成的。

| 插入和删除元素的操作                                  | 说明                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| lst.before_begin()<br />lst.cbefore_begin()           | 返回指向链表首元素之前不存在的元素迭代器。此迭代器不能解引用。cbefore_begin() 返回一个const_iterator |
| lst.insert_after(p, t)<br />lst.insert_after(p, n, t) | 在迭代器 p 之后的位置插入元素。t 是一个对象，n 是数量。      |
| lst.insert_after(p, b, e)                             | b 和 e 是表示范围的一对迭代器（b 和 e 不能指向 lst 内）      |
| lst.insert_after(p, il)                               | il 是一个花括号列表。返回一个指向最后一个插入元素的迭代器。如果范围为空，则返回p。若p为尾后迭代器，则函数行为未定义。 |
| emplace_after(p, args)                                | 使用 args 在 p 指定的位置之后创建一个元素。返回一个指向这个新元素的迭代器。若 p 为尾后迭代器，则函数行为未定义。 |
| lst.erase_after(p)<br />lst.erase_after(b, e)         | 删除 p 指向的位置之后的元素，或删除从 b 之后直到（但不包含）e 之间的元素。返回一个指向被删元素之后元素的迭代器，若不存在这样的元素，则返回尾后迭代器。如果 p 指向 lst 的尾元素或者是一个尾后迭代器，则函数行为未定义。 |

### 改变容器大小

* resize 可以增大或缩小容器。array不支持resize。
* 如果当前大小大于所要求的大小，容器后部的元素会被删除；如果当前大小小于新大小，会将新元素添加到容器后部。
* **如果 resize 缩小容器，则指向被删除元素的迭代器、引用和指针都会失效；对 vector、string 或 deque 进行 resize 可能导致迭代器、指针和引用失效。**

| 顺序容器大小操作 | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| c.resize(n)      | 调整 c 的大小为 n 个元素。若 n < c.size()，则多出的元素被丢弃。若必须添加新元素，对新元素进行值初始化。 |
| c.resize(n, t)   | 调整 c 的大小为 n 个元素。任何新添加的元素都初始化为值 t     |

### 对容器的操作可能使迭代器失效 P315

### 管理容量的成员函数

* shrink_to_fit 只适用于 vector、string 和 deque。
* capacity 和 reserve 只适用于 vector 和 string。

| 容器大小管理操作  | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| c.shrink_to_fit() | 将 capacity() 减少为与 size() 相同大小（只是一个请求，标准库并不保证退还内存） |
| c.capacity()      | 不重新分配内存的话，c 可以保存多少元素                       |
| c.reserve(n)      | 分配至少能容纳 n 个元素的内存空间（并不改变容器中元素的数量） |

reserve 并不改变容器中元素的数量，它仅影响 vector 预先分配多大的内存空间。只有当需要的内存空间超过当前容量时，reserve 调用才会改变 vector 的容量。如果需求大小大于当前容量，reserve至少分配与需求一样大的内存空间（可能更大）。

调用 reserve 永远也不会减少容器占用的内存空间。类似的，resize 成员函数只改变容器中元素的数目，而不是容器的容量。我们同样不能使用resize来减少容器预留的内存空间。



## Vector

[C++ vector的内部实现原理及基本用法](https://blog.csdn.net/lmhlmh_/article/details/80545046)

**如果你的元素是结构或是类,那么移动的同时还会进行构造和析构操作，所以性能不高 （最好将结构或类的指针放入vector中，而不是结构或类本身，这样可以避免移动时的构造与析构)。**

**增长速度**

Vector每次增长都要重新分配全部内存，而不是在原有内存尾部申请需要增加的内存。

STL使用的增长速度是1.5，理论上讲最优的增长速度是1.618（黄金分割比）。

如果使用增长速度为2，那么每一次的分配都会大于前几次分配之和，导致无法重复利用之前的内存，使用1.5可以重复利用之前的内存，~~防止了内存的浪费~~（还要具体看一下，每一都是重新分配，为什么会防止浪费？）。

> [C++ STL中vector内存用尽后，为啥每次是两倍的增长，而不是3倍或其他数值？](https://www.zhihu.com/question/36538542)
> [C++ Made Easier: How Vectors Grow](http://www.drdobbs.com/c-made-easier-how-vectors-grow/184401375)
> [What is the ideal growth rate for a dynamically allocated array?](https://stackoverflow.com/questions/1100311/what-is-the-ideal-growth-rate-for-a-dynamically-allocated-array)

### 删除容器中指定内容的元素

```c++
for (auto it = v.begin(); it != v.end();) {
	if (*it == /* */) {
		it = v.erase(it);
	} else {
		it++;
	}
}
```



## array

array可以拷贝或对象赋值操作。

```
array<int, 10> digits = { 0, 1, 2, 3, 4, 5 };
array<int, 10> copy = digits;  // 只要数组类型匹配即可
```



## 内置数组

int *pbeg = begin(ia);

int *pend = end(ia);  // p106

