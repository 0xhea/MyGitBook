# Basic

## 多线程编程

> [C语言多线程编程](https://www.cnblogs.com/zzdbullet/p/9526130.html)
>
> [C语言多线程](https://www.cnblogs.com/zzdbullet/p/9525776.html)

thread，锁，信号量

##### 线程死锁

##### 线程饥饿

​		资源在其中两个或以上线程或进程相互使用，第三方线程或进程始终得不到。



### 锁

#### 自旋锁

​		自旋锁（spinlock）：是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。

> [面试必备之深入理解自旋锁](https://blog.csdn.net/qq_34337272/article/details/81252853)
> [Linux内核同步方法——自旋锁（spin lock）](https://blog.csdn.net/hhhanpan/article/details/80624244)



## 进程和线程

> [同一进程中的线程究竟共享哪些资源](https://www.cnblogs.com/baoendemao/p/3804677.html)

每个进程都有4G的虚拟地址空间，其中3G用户空间，1G内核空间（linux），每个进程共享内核空间，每个进程都有独立的用户空间，

### 进程

#### 进程环境控制块（PEB)



### 线程

* 每个线程都有独立的栈，保存其运行状态和局部自动变量的。栈在线程开始的时候初始化，每个线程的栈互相独立，因此，栈是 thread safe 的。

### 堆栈

> [关于堆栈的讲解(我见过的最经典的)](https://blog.csdn.net/yingms/article/details/53188974)



## C

### Volatile 易变的（直接存取原始内存地址）

在读写对应数据之前会将寄存器中数据更新，之后再使用。不会进行编译优化

> [C/C++ Volatile关键词深度剖析](https://www.cnblogs.com/god-of-death/p/7852394.html)
>
> [C中的volatile用法](https://www.cnblogs.com/reality-soul/p/6140192.html)



### static

> [C语言中static关键字的作用详解，全网最透彻](https://blog.csdn.net/tr_ainiyangyang/article/details/80965574)



### static inline

> [C 语法中static 和inline联合使用](https://www.cnblogs.com/thrillerz/p/5208579.html)

