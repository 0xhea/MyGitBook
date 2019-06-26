# Container

## 1. Vector

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

* s.empty(); //如果栈为空则返回true, 否则返回false

* s.size(); //返回栈中元素的个数

* s.top(); //返回栈顶元素, 但不删除该元素

* s.pop(); //弹出栈顶元素, 但不返回其值

* s.push(); //将元素压入栈顶

