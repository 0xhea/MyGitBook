# Container

## 1. Vector

**增长速度**

STL使用的增长速度是1.5，理论上讲最优的增长速度是1.618（黄金分割比）。

如果使用增长速度为2，那么每一次的分配都会大于前几次分配之和，导致无法重复利用之前的内存，使用1.5可以重复利用之前的内存，~~防止了内存的浪费~~（还要具体看一下，每一都是重新分配，为什么会防止浪费？）。

> [C++ STL中vector内存用尽后，为啥每次是两倍的增长，而不是3倍或其他数值？](https://www.zhihu.com/question/36538542)
> [C++ Made Easier: How Vectors Grow](http://www.drdobbs.com/c-made-easier-how-vectors-grow/184401375)
> [What is the ideal growth rate for a dynamically allocated array?](https://stackoverflow.com/questions/1100311/what-is-the-ideal-growth-rate-for-a-dynamically-allocated-array)

### 删除容器中指定内容的元素

```
for (auto it = v.begin(); it != v.end();) {
	if (*it == /* */) {
		it = v.erase(it);
	} else {
		it++;
	}
}
```



## 2. Stack 栈

* ***top()才能查看元素，pop()不行***
* s.empty(); //如果栈为空则返回true, 否则返回false
* s.size(); //返回栈中元素的个数
* s.top(); //返回栈顶元素, 但不删除该元素
* s.pop(); //弹出栈顶元素, 但不返回其值
* s.push(); //将元素压入栈顶
* [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)



## 3. deque队列

[950. 按递增顺序显示卡牌](https://leetcode-cn.com/problems/reveal-cards-in-increasing-order/)