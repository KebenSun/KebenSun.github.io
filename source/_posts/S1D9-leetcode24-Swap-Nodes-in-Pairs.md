---
title: S1D9|leetcode24|Swap Nodes in Pairs
categories: 算法
tags:
  - leetcode
  - 链表
date: 2022-09-08 13:42:24
description: 每日一题第一赛季第八天
---
## 题目信息
[leetcode 24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)
## 题目分析
题目要求将链表的元素进行两两交换位置，例如1234->2143。
也是考验链表的基础操作，使用虚拟头节点，逻辑思路很简单，但是要注意各个指针的位置。
## 解题代码

1. 循环【O(n)】
~~~
public static ListNode swapPairs(ListNode head){
    ListNode dammyNode = new ListNode(0);
    dammyNode.next = head;
    ListNode prev = dammyNode;

    while(prev.next != null && prev.next.next != null){
        ListNode temp = head.next.next;
        prev.next = head.next;
        head.next.next = head;
        head.next = temp;
        prev = head;
        head = head.next;
    }
    return dammyNode.next;
}
~~~
