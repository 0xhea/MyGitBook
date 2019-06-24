# Today



## 未完成题目

https://leetcode-cn.com/problems/custom-sort-string/comments/

```c++
class Solution {
public:
    bool mycmp(char &a, char &b) {
        int i = ss.find(a);
        int j = ss.find(b);
        
        if (i == string::npos || j == string::npos) {
            return true;
        }
        
        return (i < j) ? true : false;
    }
    string customSortString(string S, string T) {
        ss = S;
        sort(T.begin(), T.end(), mycmp);
        
        return T;
    }
private:
    string ss;
};
```



## 遗留问题

**使用lamda表达式**







```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <functional>


using namespace std;

using std::placeholders::_1;
using std::placeholders::_2;
class Solution {
public:
	static bool mycmp(char &a, char &b) {
		int i = ss.find(a);
		int j = ss.find(b);

		if (i == string::npos || j == string::npos) {
			return true;
		}

		return (i < j) ? true : false;
	}

	string customSortString(string S, string T) {
		ss = S;
		//auto bound_cmp = bind(&Solution::mycmp, this, _1, _2);
		//sort(T.begin(), T.end(), bound_cmp);

		sort(T.begin(), T.end(), mycmp);
		return T;
	}
private:
	static string ss;
};
```



## 扩展

```c++
less<int>()
greater<int>()
。。。

stable_sort()
```

str.find函数返回值string::npos

[find函数用法](https://blog.csdn.net/flyyufenfei/article/details/65438665)

