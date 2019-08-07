# Android

## To-Do

1. ion_alloc在用户态分配了后内存如何使用
2. ion_system_heap_free分析
3. lru的作用



## ION

> [The Android ION memory allocator](https://lwn.net/Articles/480055/)
>
> [Integrating the ION memory allocator](https://lwn.net/Articles/565469/)
>
> [内存管理 —— ION]([http://kernel.meizu.com/memory%20management%20-%20ion.html](http://kernel.meizu.com/memory management - ion.html))

ION 的前任是 PMEM



### ion_system_heap_allocate()

`ion_system_heap_allocate(struct ion_heap *heap, struct ion_buffer *buffer, unsigned long size, unsigned long flags)`

```c
ion_system_heap_allocate();  // 通过for循环分配page
alloc_largest_available();  // 遍历orders数组，从最大的order分配
	max_order < orders[i];  // order只能越来越小，如果之前没有大块内存，后面又有了，也不能使用大块内存，原因未知？？？
alloc_buffer_page();
ion_page_pool_alloc();
    // 如果page pool中有，则直接从中取一块。并在page pool中删除。
    ion_page_pool_remove();
	// 如果page pool中没有了，则需要从伙伴系统中分配。
	alloc_pages(pool->gfp_mask, pool->order);
```



## Link

[Android Kernel 下载](https://android.googlesource.com/kernel/common.git)