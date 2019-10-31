# C

### Volatile 易变的（直接存取原始内存地址）

在读写对应数据之前会将寄存器中数据更新，之后再使用。不会进行编译优化

> [C/C++ Volatile关键词深度剖析](https://www.cnblogs.com/god-of-death/p/7852394.html)
>
> [C中的volatile用法](https://www.cnblogs.com/reality-soul/p/6140192.html)



### static

> [C语言中static关键字的作用详解，全网最透彻](https://blog.csdn.net/tr_ainiyangyang/article/details/80965574)



### static inline

> [C 语法中static 和inline联合使用](https://www.cnblogs.com/thrillerz/p/5208579.html)



## inline和define的区别

| 对比     | inline | define                                            |
| -------- | ------ | ------------------------------------------------- |
|          | 值传递 | 对等替换（参数要加上括号，++ --可能会被多次调用） |
| 处理阶段 |        | 预处理                                            |
|          |        |                                                   |



