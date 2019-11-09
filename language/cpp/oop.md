# object-oriented programming

## class



## 构造函数

### 1. 移动构造函数

> 移动构造函数适用于：使用A对象初始化B之后，A对象就不需要再使用的情况。
> [C++移动构造函数以及move语句简单介绍](https://www.cnblogs.com/qingergege/p/7607089.html) 

**move()函数**

- move函数会将指针使用后置为空值
- `vc.push_back(move(st));`  //执行后st.empty() = true

```c
//移动构造函数
inline String::String(String &&other)  
{
    m_data = move(other.m_data);//变成右值
    other.m_data = NULL;//此处置空避免被析构
}
```

移动构造函数的写法：使用浅拷贝，然后将原对象置为空。



## 析构函数

