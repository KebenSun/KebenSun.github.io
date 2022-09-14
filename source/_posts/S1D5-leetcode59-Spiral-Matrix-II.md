---
title: S1D5|leetcode59|Spiral Matrix II
categories: 算法
tags:
  - leetcode
  - 数组
date: 2022-06-29 15:35:50
description: 每日一题第一赛季第五题
---
## 题目信息
[leetcode 59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii)
## 题目分析
给一个正整数n，将1 ~ n^2顺时针放置在一个二维数组中，如图所示:
{% asset_img spiraln.jpg spiraln %}
按照正常放置逻辑进行操作，注意控制好每次循环的变量范围。
## 解题代码

1. 暴力解法【≈O(n)】
~~~
func generateMatrix(n int) [][]int {
  top, bottom := 0, n-1
  left, right := 0, n-1
  num := 1
  tar := n * n
  matrix := make([][]int, n)
  for i := 0; i < n; i++ {
    matrix[i] = make([]int, n)
  }
  for num <= tar {
    for i := left; i <= right; i++ {
      matrix[top][i] = num
      num++
    }
    top++
    for i := top; i <= bottom; i++ {
      matrix[i][right] = num
      num++
    }
    right--
    for i := right; i >= left; i-- {
      matrix[bottom][i] = num
      num++
    }
    bottom--
    for i := bottom; i >= top; i-- {
      matrix[i][left] = num
      num++
    }
    left++
  }
  return matrix
}
~~~