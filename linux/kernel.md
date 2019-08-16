# Kernel

## To-Do

1. 红黑树
   * [红黑树(一)之 原理和算法详细介绍](https://www.cnblogs.com/skywang12345/p/3245399.html)
   * rbtree.c : __rb_insert
2. UMA和NUMA
3. 驱动开发
   * [linux驱动题（含答案）](https://blog.csdn.net/ysh1042436059/article/details/86422815)
4. 内核学习
5. CGroup

#### 内核学习总结

> [tolimit](https://www.cnblogs.com/tolimit/)
>
> [Leborn_db](https://blog.csdn.net/XD_hebuters)
>
> [Frying人生](https://blog.csdn.net/ysh1042436059)
>
> [Linux内核学习总结](https://www.cnblogs.com/lalacindy/p/5440843.html)

> [Linux 内核学习的经典书籍及途径？](https://www.zhihu.com/question/19606660)
>
> [Linux内核应该怎么去学习？](https://www.zhihu.com/question/58121772)

## Kernel

### vmlinux.lds

> [vmlinux.lds.s文件分析](https://blog.csdn.net/dahailantian1/article/details/78584841)
> [vmlinux.lds的理解](https://www.veryarm.com/3733.html)



### do_initcalls()  --init/main.c

> [第3阶段——内核启动分析之start_kernel初始化函数(5)](https://www.cnblogs.com/lifexy/p/7366782.html)

​		do_initcall函数通过for循环，由`__initcall_start`开始，直到`__initcall_end`结束，依次调用识别到的初始化函数。

**do_initcalls的调用关系：**

```
init/main.c
    start_kernel
    rest_init
	kernel_thread(kernel_init, NULL, CLONE_FS);
	kernel_init_freeable
	do_basic_setup();
	do_initcalls();
	do_initcall_level(level);
	do_one_initcall(*fn);
```



### __define_initcall(fn, id)  --include/linux/init.h

```c
#define __define_initcall(fn, id) \
	static initcall_t __initcall_name(fn, id) __used \
	__attribute__((__section__(".initcall" #id ".init"))) = fn;
```

* `typedef int (*initcall_t)(void);`  initcall_t是一个参数为空，返回值为int的函数指针。
* `#define __initcall_name(fn, id) __initcall_##fn##id` 定义一个函数指针名。
* `__used`（GCC编译器指令）告诉编译器无论 GCC 是否发现这个函数的调用实例，都要使用这个函数。
* `__attribute__((__section__("")))` 是为链接器指定链接的section，这里按照id将各个初始化函数放置到不同的.initcallX.init段中。
  * [利用__attribute__((section()))构建初始化函数表与Linux内核init的实现](https://mp.weixin.qq.com/s?__biz=MzAwMDUwNDgxOA==&mid=2652663356&idx=1&sn=779762953029c0e0946c22ef2bb0b754&chksm=810f28a1b678a1b747520ba3ee47c9ed2e8ccb89ac27075e2d069237c13974aa43537bff4fba&mpshare=1&scene=1&srcid=0111Ys4k5rkBto22dLokVT5A&pass_ticket=bGNWMdGEbb0307Tm%2Ba%2FzAKZjWKsImCYqUlDUYPZYkLgU061qPsHFESXlJj%2Fyx3VM#rd)
  * `__attribute__((section("name")))`是gcc编译器支持的一个编译特性（arm编译器也支持此特性），实现在编译时把某个函数/数据放到name的数据段中。
  * `-T $(vmlinux-lds)`内核最顶层Makefile中使用自定义的链接脚本。
  * `vmlinux-lds := arch/$(SRCARCH)/kernel/vmlinux.lds`
* 最后将初始化函数指针赋值给了新定义的函数指针。



### subsys_initcall / module_init

> [linux模块(module_init)、子系统(subsys_initcall)入口函数详解](https://blog.csdn.net/asklw/article/details/79698422)

​		subsys_initcall 和 module_init 都会在kernel启动时被 do_initcalls() 调用。

```
#define subsys_initcall(fn)		__define_initcall(fn, 4)  // subsys_initcall
#define device_initcall(fn)		__define_initcall(fn, 6)  // module_init

#define module_init(x)    __initcall(x)
#define __initcall(fn) device_initcall(fn)
```



### EXPORT_SYMBOL()

​		EXPORT_SYMBOL标签内定义的函数或者符号对全部内核代码公开，不用修改内核代码就可以在您的内核模块中直接调用，即使用EXPORT_SYMBOL可以将一个函数以符号的方式导出给其他模块使用。

**使用方法**

1. 在模块函数定义之后使用“EXPORT_SYMBOL（函数名）”来声明。
2. 在调用该函数的另外一个模块中使用extern对之声明。
3. 先加载定义该函数的模块，然后再加载调用该函数的模块，请注意这个先后顺序。



### 注册 platform 驱动

> [[驱动注册]platform_driver_register()与platform_device_register()](https://blog.csdn.net/ufo714/article/details/8595021)

**步骤：**

1. 注册设备platform_device_register
2. 注册驱动platform_driver_register，过程中在系统寻找注册设备（根据.name)，找到后运行.probe进行初始化。

#### platform_device_register()

​		platform_device_系列函数，实际上是注册了一个叫platform的虚拟总线。使用约定是如果一个不属于任何总线的设备，例如蓝牙，串口等设备，都需要挂在这个虚拟总线上。

#### platform_driver_register()

​		在驱动程序的初始化函数中，调用了platform_driver_register() 注册 platform_driver。需要注意的是：**platform_driver 和 platform_device 中的 name 变量的值必须是相同的** 。这样在 platform_driver_register（） 注册时，会将当前注册的 platform_driver 中的 name 变量的值和已注册的所有 platform_device 中的 name 变量的值进行比较，只有找到具有相同名称的 platform_driver 才能注册成功。**当注册成功时，会调用 platform_driver 结构元素 probe 函数指针**。

**注册过程：**

```
platform_driver_register(struct platform_driver drv)
__platform_driver_register(drv, THIS_MODULE)
driver_register(&drv->driver);
bus_add_driver(drv);
driver_attach(drv);
bus_for_each_dev(drv->bus, NULL, drv, __driver_attach);
error = fn(dev, data);  // 实际调用 __driver_attach
driver_probe_device(drv, dev);
really_probe(dev, drv);
dev->bus->probe(dev); || drv->probe(dev);  // 最后调用probe
```



### misc_register

???



### container_of(ptr,type,member)

> [container of()函数简介](https://blog.csdn.net/s2603898260/article/details/79371024)

​		已知结构体type的成员member的地址ptr，求解结构体type的起始地址。

```
#define container_of(ptr, type, member) ({			\
	const typeof( ((type *)0)->member ) *__mptr = (ptr);	\
	(type *)( (char *)__mptr - offsetof(type,member) );})

#define offsetof(TYPE, MEMBER)	((size_t)&((TYPE *)0)->MEMBER)
```

* typeof( ((type *)0)->member )：获取member的类型，typeof 是 C 语言的关键字。
* offsetof 计算 MEMBER 成员的偏移



### Kconfig

​		Kconfig 服务于 `make menuconfig` 时出现的内核配置界面




### 一些宏

* **__user** 表明参数是一个用户空间的指针，不能在 kernel 代码中直接访问。也方便其它工具对代码进行检查。

```
#ifdef __CHECKER__
# define __user __attribute__((noderef, address_space(1)))
# define __kernel /* default address space */
#else
# define __user
# define __kernel
#endif
```

__used

__init

`__attribute__`

[C语言中`__attribute__`的用法](https://wenku.baidu.com/view/4016dcc55ebfc77da26925c52cc58bd6318693ab.html)

__start

[Linux内核 __setup宏分析](https://blog.csdn.net/zhangdaxia2/article/details/83177496)



### 为什么部分代码是汇编语言编写的

1. linux内核中的底层程序直接与硬件打交道，需要一些专用的指令，而这些指令在C语言中并无对应的语言成分。
2. 内核中实现某些操作的过程、程序段或函数，在运行时会非常频繁的被调用，这时用汇编语言编写，其时间效率会有大幅度提高。
3. 在某些特殊的场合，一段程序的空间效率也非常重要，比如操作系统的引导程序一定要容纳在磁盘的第一个扇区中，多一个字节都不行。这时只能用汇编语言编写。




## 数据结构

### klist

> [linux内核源代码分析----内核基础设施之klist](https://blog.csdn.net/qq_21435127/article/details/80805195)

​		 klist是list的线程安全版本，他提供了整个链表的自旋锁，查找链表节点，对链表节点的插入和删除操作都要获得这个自旋锁。klist的节点数据结构是klist_node,klist_node引入引用计数，只有点引用计数减到0时才允许该node从链表中移除。当一个内核线程要移除一个node，必须要等待到node的引用计数释放，在此期间线程处于休眠状态，为了方便线程等待，klist引入等待移除节点者结构体klist_waiter，klist-waiter组成klist_remove_waiters（内核全局变量）链表，为保护klist_remove_waiters线程安全，引入klist_remove_lock（内核全局变量）自旋锁。为方便遍历klist，引入了迭代器klist_iter。

### list

[list.h接口说明](https://blog.csdn.net/lmjjw/article/details/9833025)



bigmem: 一种内核的模式\或是1G 以上内存的优化内核

smp:    多处理器的模式\该模式可以打开多个CPU的支持\也可以打开P4超线程技术

up:     单处理器的模式



[Linux内核源代码分析工具](http://www.itboth.com/d/ZbmeEr/linux)