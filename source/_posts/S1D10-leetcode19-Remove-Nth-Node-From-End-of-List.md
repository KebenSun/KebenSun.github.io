---
title: S1D10|leetcode19|Remove Nth Node From End of List
categories: 算法
tags:
  - leetcode
  - 链表
date: 2022-09-08 15:21:35
description: 每日一题第一赛季第十题
---
## 题目信息
[leetcode 19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
## 题目分析
题目要求删除链表的倒数指定节点的元素，并将删除后的链表头返回，例如123456删除倒数第2个，则返回12346。
本题采用双指针的方式处理，快的指针比慢的指针快n个节点，当快的指针到达链表末尾时，将慢的指针元素移除。
同时考虑可能会存在移除第一个节点的情况，所以采用虚拟头节点，更方便处理。
## 解题代码

1. 循环【O(n)】
~~~
public static ListNode removeNthFromEnd (ListNode head, int n){
    ListNode dammyNode = new ListNode(0);
    dammyNode.next = head;
    ListNode fast = head;
    ListNode slow = dammyNode;
    while(fast!=null){
        if(n<=0){
            slow = slow.next;
        }
        if(fast.next == null){
            slow.next = slow.next.next;
        }
        n--;
        fast = fast.next;
    }
    return dammyNode.next;
}
~~~
