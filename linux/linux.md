# Linux

## Kernel

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







## 加解压

* [linux tar.gz zip 解压缩 压缩命令](https://www.cnblogs.com/wangluochong/p/7194037.html)

1. \*.tar 用 tar –xvf 解压
2. \*.gz 用 gzip -d或者gunzip 解压
3. \*.tar.gz和\*.tgz 用 tar –xzf 解压
4. \*.bz2 用 bzip2 -d或者用bunzip2 解压
5. \*.tar.bz2用tar –xjf 解压
6. \*.Z 用 uncompress 解压
7. \*.tar.Z 用tar –xZf 解压
8. \*.rar 用 unrar e解压
9. \*.zip 用 unzip 解压

