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

​		分配好一个 page 后，链接 page->lru 到 pages，pages最终是所有分配页的总链表。之遍历 pages 将所有 page 保存到 **buffer->sg_table** 中。

### lru in ION

​		pool 中的 high_items 和 low_items 分别是一个 page 的 lru 链表。分配时，将 page 从链表中摘除，并将 lru 的前后指针设置为 LIST_POISON2 / LIST_POISON1。

### page in ION

​		order > 0 的内存块是由多个 page （2^order）组成的，每次对首个 page 的操作相当于对所有 page 一起进行了操作，也就是说每次操作所有 page 相当于一个对象。例如，从 pool 中移除节点只需要移除首个page的lru指针。

### ion_system_heap_free()

```
ion_heap_buffer_zero(buffer);  // 将所有page中的内容置为0（循环了每个内存块的每个page）
free_buffer_page();  // 通过for循环遍历所有 page
ion_page_pool_free();
ion_page_pool_add();  // 将page->lru添加到high_items或low_items
```

​		ion_system_heap_free() 会将所有释放的内存块挂载到 pool 中，不会进行释放。挂载第一个 page 就相当于将整个内存块挂载到 pool 中，因为内存块中的 page 都是连续的。

### ion_system_heap_shrink()

`static int ion_system_heap_shrink(struct ion_heap *heap, gfp_t gfp_mask, int nr_to_scan)`

* gfp_mask: 内存分配掩码，用于判断是否是高端域内存
* nr_to_scan: 如果为0，则统计 pool 中剩余的块数量；如果不为0，则为应该释放的 (struct) page 数。

```c
// 只分析nr_to_scan不为0的情况
ion_system_heap_shrink();  // 按order循环，大块内存先释放
ion_page_pool_shrink();  // 通过for循环释放指定pool的page，直至pool为空或已经释放足够的大小
ion_page_pool_remove();  // 从pool中取下一块内存块，并在pool中删除
ion_page_pool_free_pages();
__free_pages(page, pool->order);  // 释放page到伙伴系统中
```



## Link

[Android Kernel 下载](https://android.googlesource.com/kernel/common.git)