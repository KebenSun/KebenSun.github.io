---
title: S1D2|leetcode27|Remove Element
date: 2022-06-24 09:46:09
description: 每日一题第一赛季第二天
categories: 算法
tags:
- leetcode
- 数组
---
## 题目信息
[leetcode 27. Remove Element](https://leetcode.com/problems/remove-element)
## 题目分析
给定一个数组，移除指定内容的元素，剩余的元素要放在结果数组的前面。
因为数组的长度是固定的，所以暴力解法中，移除数组要循环一次，挪动后续元素的位置又要循环一次，很容易就做成了O(n^2)。
借鉴网上的解法，使用双指针方法。双指针同时从下标0出发，慢的指针负责存储结果，只有当不等于目标值的元素时，才会放置在慢指针下标处，这样也就避免了挪动元素的二次循环，快的指针负责遍历整个数组，确保所有元素都被校验过。
另外题目中提到，空间要求O(1)，也就是不能开辟新的数组，不过用双指针法，两个指针操作的是给定的数据，这个要求也满足。
## 解题代码

1. 暴力解法【O(n^2)】
~~~
func removeElement(nums []int, val int) int {
	k := 0
	size := len(nums)
	for i := 0; i < size; i++ {
		if nums[i] == val {
			k++
			for j := i + 1; j < size; j++ {
				nums[j-1] = nums[j]
			}
			i--
		}
	}
	return len(nums) - k
}
~~~

2. 双指针法【O(n)】
~~~
func removeElement(nums []int, val int) int {
	res := 0
	for i := 0; i < len(nums); i++ {
		if nums[i] != val {
			nums[res] = nums[i]
			res++
		}
	}
	return res
}
~~~