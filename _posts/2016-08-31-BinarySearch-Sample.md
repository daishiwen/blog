---
layout: post
category: 技术
tags: [Java, 示例, 算法]
title: 二分查找算法
header: no
---

摘自[二分查找算法java实现](http://www.cnblogs.com/vanezkw/archive/2012/06/29/2569470.html)

### 概述

二分查找算法：也称为折半搜索、二分搜索，是一种在有序数组中查找某一特定元素的搜索算法。

### 实现思路

1. 找出数组的中间元素，并存放在一个变量temp中；  
2. 用需要搜索的元素key和temp做比较；  
3. 如果key>temp则把数组的中间位置作为下次搜索的起点，然后重复1.2步；  
4. 如果key<temp则把数组的中间位置作为下次搜索的终点，然后重复1.2.3步；  
5. 如果key=temp则返回数组下标，完成搜索。

### 实现代码

- 递归方式

{% highlight java %}
public static <E extends Comparable<E>> int binarySearch(E[] array, int from, int to, E key) throws Exception {
	if (from < 0 || to < 0) {
		throw new IllegalArgumentException("params from & length must larger than 0 .");
	}
	if (from <= to) {
		int middle = (from >>> 1) + (to >>> 1); // 右移即除2
		E temp = array[middle];
		if (temp.compareTo(key) > 0) {
			to = middle - 1;
		} else if (temp.compareTo(key) < 0) {
			from = middle + 1;
		} else {
			return middle;
		}
	}
	return binarySearch(array, from, to, key);
}
{% endhighlight %}

- 非递归方式，摘自JDK

{% highlight java %}
private static int binarySearch0(int[] a, int fromIndex, int toIndex, int key) {
	int low = fromIndex;
	int high = toIndex - 1;
	while (low <= high) {
		int mid = (low + high) >>> 1;
		int midVal = a[mid];
		if (midVal < key)
			low = mid + 1;
		else if (midVal > key)
			high = mid - 1;
		else
			return mid; // key found
	}
	return -(low + 1);  // key not found.
}
{% endhighlight %}
