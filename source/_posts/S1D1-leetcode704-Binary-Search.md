---
title: S1D1|leetcode704|Binary Search
date: 2022-06-23 14:34:46
categories: 
- 算法
tags: 
- leetcode
- 数组
description: 每日一题第一赛季第一天
---
## 题目信息
[leetcode 704. Binary Search](https://leetcode.com/problems/binary-search)
## 题目分析
有序且不重复的数组，查找指定元素，与题目的名字一样，标准的二分查找法。
使用二分查找法时，需要着重注意边界问题，不然很容易一锅粥。
参考网上的解题思路，分为两种方式：
1. 左闭右开
循环条件：left \< right
         因为left == right在区间[left, right)是没有意义的
边界替换：if (nums[middle] > target) right = middle
2. 左闭右闭
循环条件：left \<= right
         因为left == right 在区间[left, right)是有意义的
边界替换：if (nums[middle] > target) right = middle - 1
         因为middle肯定不是target
## 解题代码

1. 暴力枚举版【O(n)】
~~~
func search(nums []int, target int) int {
	for i, v := range nums {
		if v == target {
			return i
		}
	}
	return -1
}
~~~

2. 二分查找-左闭右闭版【O(log n)】
~~~
func search(nums []int, target int) int {
	left := 0
	right := len(nums) - 1
	for left <= right {
		mid := left + (right-left)/2
		if nums[mid] > target {
			right = mid - 1
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			return mid
		}
	}
	return -1
}
~~~

3. 二分查找-左闭右开版【O(log n)】
~~~
func search(nums []int, target int) int {
    right := len(nums)
    left := 0
    for left < right {
        mid := left + (right-left)/2
        if nums[mid] == target {
            return mid
        } else if nums[mid] > target {
            right = mid
        } else {
            left = mid+1
        }
    }
    return -1
}
~~~