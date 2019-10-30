# Tool

### valgrind

Valgrind是一套Linux下，开放源代码（GPL V2）的仿真调试工具的集合。Valgrind由内核（core）以及基于内核的其他调试工具组成。内核类似于一个框架（framework），它模拟了一个CPU环境，并提供服务给其他工具；而其他工具则类似于插件 (plug-in)，利用内核提供的服务完成各种特定的内存调试任务。

可以用来检测内存是否有异常。

### readelf -l/--all [ELF]

读取ELF（c语言编译形成的二进制文件）的信息。

### ldd [ELF]

查看链接库

### objdump -d [ELF]

反编译ELF文件

### pmap [pid] / cat /proc/pid/maps

查看进程内存映射情况

### Bazel

> [[bazel]-bazel的使用](https://www.jianshu.com/p/543ced50a566)
>
> [Google软件构建工具Bazel原理及使用方法介绍](https://www.cnblogs.com/Jack47/p/build-in-the-cloud.html)



> [Android NDK开发Crash错误定位](https://blog.csdn.net/xyang81/article/details/42319789)
>
> [arm-eabi-addr2line android应用崩溃的调试方法](https://blog.csdn.net/tommy_wxie/article/details/12841735)

