# Debug

### GCC



### GDB

```
run
list
info line
disassemble
print
display
jump
signal <signal>  // 产生一个信号量，发送给程序，通常是1~15
return [expression]  // 强制函数返回，如果指定了expression，expression作为函数返回值
call expr  // 强制调用函数，并显示函数的返回值（print expr有相同功能）
```



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



