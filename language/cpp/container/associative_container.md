# 关联容器

| 关联容器类型                         | 说明                                   |
| ------------------------------------ | -------------------------------------- |
| **按关键字有序保存元素**             |                                        |
| map `<map>`                          | 关联数组；保存 关键字-值(key-value) 对 |
| set `<set>`                          | 关键字即值，即只保存关键字的容器       |
| multimap `<map>`                     | 关键字可重复出现的 map                 |
| multiset `<set>`                     | 关键字可重复出现的 set                 |
| **无序集合**                         |                                        |
| unordered_map`<unordered_map>`       | 用哈希函数组织的 map                   |
| unordered_set `<unordered_set>`      | 用哈希函数组织的 set                   |
| unordered_multimap`<unordered_map>`  | 哈希组织的 map；关键字可以重复出现     |
| unordered_multiset `<unordered_set>` | 哈希组织的 set；关键字可以重复出现     |

## 使用关联容器

### 使用 map

```c++
map <string, size_t> word_count;
string word;
while (cin >> word)
	++word_count[word];
for (const auto &w : word_count)
	cout << w.first << " occurs " << w.second
    	 << ((w.second > 1) ? " times" : " time") << endl;
```

待补充

### 使用 set

```c++
map<string, size_t> word_count;
set<string> exclude = { "The", "But", "And", "Or", "An", "A",
                        "the", "but", "and", "or", "an", "A" };
string word;
whild (cin >> word)
    if (exclude.find(word) == exclude.end())
        ++word_count[word];
```



## 关联容器概述

### 定义关联容器





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



## Hash_Map

通过hash表存储

需要实现hash函数、比较函数（等与函数）

