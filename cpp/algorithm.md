# Algorithm

## 1. 最长回文子串

[5.最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

1. 中心扩展算法
2. 马拉车算法

## 2. KMP

[很详尽KMP算法（厉害）](https://www.cnblogs.com/ZuoAndFutureGirl/p/9028287.html)



## 2.回溯算法

回溯算法实际上一个类似枚举的搜索尝试过程，主要是在搜索尝试过程中寻找问题的解，当发现已不满足求解条件时，就“回溯”返回，尝试别的路径。回溯法是一种选优搜索法，按选优条件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。许多复杂的，规模较大的问题都可以使用回溯法，有“通用解题方法”的美称。

[小白带你学--回溯算法](https://www.jianshu.com/p/dd3c3f3e84c0)



## 3. 汉明距离

* [1051. 高度检查器](https://leetcode-cn.com/problems/height-checker/)



## 4. 排序

### 桶排序

[算法：排序算法之桶排序](https://blog.csdn.net/developer1024/article/details/79770240)

[拜托，面试别再问我桶排序了！！！](https://blog.csdn.net/z50L2O08e2u4afToR9A/article/details/83513673)



## 思路

* [73. 矩阵置零](https://leetcode-cn.com/problems/set-matrix-zeroes/)：通过原地存放标志位解决额外空间问题

* [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)：瓶颈为短的

* 迷宫问题，一般就提示我们回溯+DFS（深度优先搜索）

### String

1. 判断两字符串组成字母是否相同，先排序
2. 判断括号是否匹配用stack
   - [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

