# Algorithms

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

### 冒泡排序

```c
void bubbleSort(int* nums, int numsSize) {
	int i, j, temp;
	for (i = 0; i < numsSize - 1; i++) {
		for (j = 1; j < numsSize - i; j++) {
			if (nums[j - 1] > nums[j]) {
				temp = nums[j - 1];
				nums[j - 1] = nums[j];
				nums[j] = temp;
			}
		}
	}
}
```

### 选择排序

每次选择区间内最小的，放在区间前面。

```c
void selectSort(int* nums, int numsSize) {
	int i, j, min;
	for (i = 0; i < numsSize - 1; i++) {
		for (min = i, j = i + 1; j < numsSize; j++) {
			if (nums[j] < nums[min]) {
				min = j;
			}
		}
		swap(&nums[min], &nums[i]);
	}
}
```

### 插入排序

将后面的数字插入到之前已经排好的序列中。

```c
void insertSort(int* nums, int numsSize) {
	int i, j, k, temp;
	for (i = 1; i < numsSize; i++) {
		for (j = i; j > 0 && nums[j] < nums[j - 1]; j--) {
			swap(&nums[j], &nums[j - 1]);
		}
	}
}
```

### 快排

选择基准，将大于基准的放在右侧，小于基准的放在左侧，然后递归，分而治之。

```c
void quickSort(int *nums, int begin, int end) {
	int b = begin;
	int e = end;
	int flag = nums[begin];

	if (begin >= end) {
		return;
	}

	while (b < e) {
		while (e > b && nums[e] >= flag) e--;
		nums[b] = nums[e];

		while (b < e && nums[b] <= flag) b++;
		nums[e] = nums[b];
	}
	nums[b] = flag;

	quickSort(nums, begin, b - 1);
	quickSort(nums, e + 1, end);
}

int* sortArray(int* nums, int numsSize, int* returnSize) {
	quickSort(nums, 0, numsSize - 1);
	*returnSize = numsSize;

	return nums;
}
```

### 归并排序

通过递归，不断地将两个排好序的数组合并成一个数组。

```
void marge(int* nums, int left, int mid, int right) {
	int left_size = mid - left + 1;
	int right_size = right - mid;
	int *left_arr, *right_arr;
	int i, j, k;

	left_arr = (int *)malloc(left_size * sizeof(int));
	right_arr = (int *)malloc(right_size * sizeof(int));

	for (i = 0; i < left_size; i++) {
		left_arr[i] = nums[left + i];
	}
	for (i = 0; i < right_size; i++) {
		right_arr[i] = nums[mid + 1 + i];
	}

	i = 0;
	j = 0;
	k = left;
	while (i < left_size && j < right_size) {
		if (left_arr[i] < right_arr[j]) {
			nums[k++] = left_arr[i++];
		}
		else {
			nums[k++] = right_arr[j++];
		}
	}

	while (i < left_size) {
		nums[k++] = left_arr[i++];
	}
	while (j < right_size) {
		nums[k++] = right_arr[j++];
	}

	free(left_arr);
	free(right_arr);
}

void margeSort(int *nums, int left, int right) {
	if (left == right) {
		return;
	}

	int mid = (left + right) / 2;

	margeSort(nums, left, mid);
	margeSort(nums, mid + 1, right);
	marge(nums, left, mid, right);
}
```

### 希尔排序

增量不断缩小的的插入排序。

### 堆排序

堆排序是选择排序的改进。

最大堆

```
void heapify(int *nums, int i, int n) {
	if (2 * i + 1 >= n) {
		return;
	}

	int child = 2 * i + 1;

	if (2 * i + 2 < n && nums[child] < nums[child + 1]) {
		child++;
	}

	if (nums[i] < nums[child]) {
		swap(&nums[i], &nums[child]);
	}

	heapify(nums, child, n);
}

void heapSort(int *nums, int numsSize) {
	int i;
	for (i = (numsSize - 2) / 2; i >= 0; i--) {
		heapify(nums, i, numsSize);
		//printArr(nums, numsSize);
	}

	for (i = numsSize - 1; i > 0; i--) {
		swap(&nums[0], &nums[i]);
		heapify(nums, 0, i);
	}
}
```



### 桶排序

[算法：排序算法之桶排序](https://blog.csdn.net/developer1024/article/details/79770240)

[拜托，面试别再问我桶排序了！！！](https://blog.csdn.net/z50L2O08e2u4afToR9A/article/details/83513673)



![img](https://img-blog.csdn.net/20180912224019565?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NzcwNjQx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



## 思路

* [73. 矩阵置零](https://leetcode-cn.com/problems/set-matrix-zeroes/)：通过原地存放标志位解决额外空间问题

* [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)：瓶颈为短的

* 迷宫问题，一般就提示我们回溯+DFS（深度优先搜索）

### String

1. 判断两字符串组成字母是否相同，先排序
2. 判断括号是否匹配用stack
   - [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

