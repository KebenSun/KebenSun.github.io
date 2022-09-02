---
title: S1D8|leetcode206|Reverse Linked List
categories: 算法
tags:
  - leetcode
  - 链表
date: 2022-09-02 14:08:53
description: 每日一题第一赛季第八天
---
## 题目信息
[leetcode 206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
## 题目分析
题目要求将一个单链表进行反转操作，还是考验对链表的基础操作。
为了节省内存空间，所以在原链表上进行处理，将链表每个节点的指向方向转换一下即可。
注意转换之前要将节点的next暂存一下
## 解题代码

1. 指针解法【O(n)】
~~~
    public static ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode temp = null;
        while (cur != null) {
            temp = cur.next;
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
~~~

2. 递归解法【O(n)】
~~~
    public static ListNode reverseList(ListNode head) {
        return reverse(null, head);
    }

    public static ListNode reverse(ListNode pre, ListNode cur){
        if(cur == null){
            return pre;
        }
        ListNode temp = cur.next;
        cur.next = pre;
        pre = cur;
        cur = temp;
        return reverse(pre, cur);
    }
~~~