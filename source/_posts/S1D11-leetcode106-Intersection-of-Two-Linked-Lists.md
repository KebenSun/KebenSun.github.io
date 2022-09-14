---
title: S1D11|leetcode106|Intersection of Two Linked Lists
categories: 算法
tags:
  - leetcode
  - 链表
date: 2022-09-14 10:28:24
description: 每日一题第一赛季第十一题
---
## 题目信息
[leetcode 106. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)
## 题目分析
给到两个链表，查找两个链表从哪个节点开始相同，注意此处相同是指引用相同，不是值相同。
简单的暴力解法，两层循环嵌套，找到相等的节点，就是结果。
因为链表的特殊性，当找到相同的一个节点时，他的后续节点也必然相同，所以相同的节点，总是位于链表的倒数相同的位置。所以利用此特性，可以先将较长一个的链表，提前移动至与较短链表相同长度的节点，然后同时开始遍历里两个链表，查找是否有相同的节点。
## 解题代码

1. 暴力解法【O(m*n)】
~~~
public static ListNode getIntersectionNode1(ListNode headA, ListNode headB) {
    ListNode nodeA = headA;
    while(nodeA != null){
        ListNode nodeB = headB;
        while(nodeB != null){
            if(nodeA == nodeB){
                return nodeA;
            }
            nodeB = nodeB.next;
        }
        nodeA = nodeA.next;
    }
    return new ListNode();
}
~~~

2. 对齐后查找【O(m+n)】
~~~
public static ListNode getIntersectionNode2(ListNode headA, ListNode headB){
    ListNode curA = headA;
    ListNode curB = headB;
    int lenA = 0, lenB = 0;

    while(curA != null){
        lenA++;
        curA = curA.next;
    }
    curA = headA;
    while(curB != null){
        lenB++;
        curB = curB.next;
    }
    curB = headB;
    if(lenA < lenB){
        int tempLen = lenA;
        lenA = lenB;
        lenB = tempLen;
        ListNode tempCur = curA;
        curA = curB;
        curB = tempCur;
    }

    int gap = lenA = lenB;
    while(gap-- > 0){
        curA = curA.next;
    }

    while(curA != null){
        if(curA == curB){
            return curA;
        }
        curA = curA.next;
        curB = curB.next;
    }

    return null;
}
~~~