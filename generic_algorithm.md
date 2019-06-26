# 泛型算法

**全局函数无法调用非静态成员函数**  [不能使用非静态成员函数说明](https://blog.csdn.net/lym940928/article/details/89353485)

* 可以使用bind调用非静态成员函数  [bind函数](https://ask.csdn.net/questions/259500)

## 1. 排序

### 1. Sort

* [791. 自定义字符串排序](https://leetcode-cn.com/problems/custom-sort-string/)  sort/lamda表达式

### 2. stable_sort

* [937. 重新排列日志文件](https://leetcode-cn.com/problems/reorder-log-files/)



***

## 向算法传递函数

1. Sort
    * **升序** `sort(arr,arr+len,less<>(type));`
    * **降序** `sort(arr,arr+len,greater<>(type));`

