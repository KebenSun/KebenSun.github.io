---
title: S1D3|leetcode977|Squares of a Sorted Array
categories: 算法
tags:
  - leetcode
  - 数组
date: 2022-06-27 15:21:33
description: 每日一题第一赛季第三天
---
## 题目信息
[leetcode 977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)
## 题目分析
给一个数组，非降序排序。要求将数组每个元素平方之后排序，返回排序后的数组。
暴力解法是先将数据循环平方，然后再对数据进行排序。
但是题目也有补充，对每个元素进行平方并对新数组进行排序非常简单，能否找到使用不同方法的 O(n) 解决方案。
参考网上解题思路，直接对数组的平方排序，然后放入新的数组。因为非降序数组平方之后，元素有可能是递增的，也有可能是两边大，中间小。所以排序方式采用两边向中间的比较方式。因为是从大到小筛选，所以放置到新的数组时，是倒叙放置。
## 解题代码
1. 解法【O(n)】
~~~
func squaresAndSort(nums []int) []int {
  length := len(nums)
  res := make([]int, length)
  k := length - 1
  i := 0
  j := length - 1

  for i <= j {
    lm := nums[i] * nums[i]
    rm := nums[j] * nums[j]
    if lm <= rm {
      res[k] = rm
      j--
    } else if lm > rm {
      res[k] = lm
      i++
    }
    k--
  }
  return res
}
~~~