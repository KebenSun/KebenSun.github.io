---
title: S1D4|leetcode209|Minimum Size Subarray Sum
categories: 算法
tags:
  - leetcode
  - 数组
date: 2022-06-28 14:40:07
description: 每日一题第一赛季第四天
---
## 题目信息
[leetcode 209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
## 题目分析
给一个非负整数数组，给一个非负整数，找出大于等于非负整数的最小数组片段。
暴力解法是两次循环，第一层选定开始元素，第二层循环从开始元素之后的一个，不断累加，看有没有符合条件的片段，此方法时间复杂度O(n^2)
使用双指针方法，可以O(n)解决。具体思路是定义两个指针，第一个指针指向当前片段的结束元素。虽然和暴力解法思路差不多，都是找到一个开始元素，然后去判断后续的片段，但不一样的在于当片段符合条件之后，不会重置第一个指针的下表，而是直接向后移动一个，第二个指针也是从上一次结束的位置开始判断。这样虽然是两重循环嵌套，但是每个元素操作次数是两次，也就是2\*n，时间复杂度O(n)。
## 解题代码
1. 双指针解法【O(n)】
~~~
func minSubArrayLen(target int, nums []int) int {
    i := 0
    l := len(nums)  
    sum := 0        
    result := l + 1 
    for j := 0; j < l; j++ {
        sum += nums[j]
        for sum >= target {
            subLength := j - i + 1
            if subLength < result {
                result = subLength
            }
            sum -= nums[i]
            i++
        }
    }
    if result == l+1 {
        return 0
    } else {
        return result
    }
}
~~~