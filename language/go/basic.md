# GO

## 基本数据类型



## 占位符



## 运算符

++ -- 是单独的语句



## 数组（Array）

```go
var a1 [3]bool
```

### 初始化

```go
a1 := [4]int{1, 2, 3, 4}
a2 := [...]int{1, 2, 3, 4, 5, 6}  // 推断数组长度
a3 := [5]int{0: 1, 2: 3, 4}  // 根据索引值[1 0 3 4 0]
```

### 遍历

```go
for i := 0; i < len(a3); i++ {
    fmt.Println(a3[i])
}

for i, v := range a3 {
    fmt.Println(i, v)
}
```

### 多维数组

```go
a1 := [3][2]int{{1, 2}, {3, 4}, {5, 6}}
```



## 切片（slice）

引用类型，不能比较

```go
var s1 []int
var s2 []int{1, 2, 3}
```

nil：空

### 由数组/切片得到切片

```go
a1 := [...]int{1, 3, 5, 7, 9}
s1 := a1[0:4]  // 左闭右开
s1 := a1[1:]
s1 := a1[:4]
s1 := a1[:]
```

### make函数创建切片

```go
s1 := make([]int, 5, 10)  // len:5, cap:10
```

### append函数追加元素

append函数返回值必须用变量接收

```go
a1 = append(a1, 10)
a1 = append(a1, 10, 12, 13)
a2 = append(a2, a1...)  // ...表示拆开，将a1中所有数据添加到a2中
```

### copy函数拷贝切片

使用 copy(a, b) 函数将切片b中的元素**复制**（非引用）到切片a中。

```go
a1 := []int{1, 2, 3}
a2 := make([]int, 3, 3)
copy(a2, a1)
fmt.Println(a2)  // [1 2 3]
```

### 删除切片中的元素

```go
a1 = append(a1[:2], a1[3:]...)  // 删除下标为2的元素，拼接
```



## 函数

```go
func plus(x int, y int) int {  // 匿名返回值
    return x + y
}

func plus(x int, y int) (ret int) {  // 命名返回值
    ret = x + y
    return  // 使用命名返回值，return后可省略
}

func f1() (int, string) {  // 多个返回值
    return 10, "abc"
}

func plus(x, y, z int, m, n string) int {  // 类型一致的形参除最后一个可以省略类型
    return x + y + z, m, n
}

func f2(x string, y ...int) {  // 可变长参数，y的类型是一个切片（类型是int）
    fmt.Println(x, y)
}
```

* 返回值可以命名也可以不命名，命名了相当于在函数中声明了一个变量
* 函数可以有多个返回值，函数如果有多个返回值时，所有返回值必须用`()`包裹起来
* 当形参中连续的多个参数的类型一致时，可以省略除最后一个形参的类型
* 可变长参数必须放在函数参数的最后
* **Go语言中函数没有默认参数这个概念**

### defer 语句

Go语言中的`defer`语句会将其后面跟随的语句进行延迟处理。在`defer`归属的函数即将返回时，将延迟处理的语句按`defer`定义的逆序进行执行，也就是说，先被`defer`的语句最后被执行，最后被`defer`的语句，最先被执行。

### 函数类型

函数可以作为参数，也可以作为返回值。

### 匿名函数

没有名字的函数。



## 自定义类型/类型别名

```go
type 新类型名 旧类型名

type myInt int     // 自定义类型
type myInt2 = int  // 类型别名
```



## 结构体

值类型

```go
type person struct {
    name string
    age int
    hobby []string
}
```

### 方法



## 接口（interface）

接口是一种类型

