# 关联容器

## Map

通过**红黑树**存储查找

**遍历**

```
for(auto iter=maps.begin(); iter!=maps.end(); iter++)
{
	int i = iter->first;
	string s = iter->second;
}
// 也可以通过下标遍历 string s = maps[i];
```



## Set



## Hash_Map

通过hash表存储

需要实现hash函数、比较函数（等与函数）

