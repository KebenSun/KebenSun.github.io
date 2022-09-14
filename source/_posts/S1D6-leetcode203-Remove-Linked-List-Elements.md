---
title: S1D6|leetcode203|Remove Linked List Elements
categories: 算法
tags:
  - leetcode
  - 链表
date: 2022-07-27 15:53:43
description: 每日一题第一赛季第六题
---
## 题目信息
[leetcode 203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)
## 题目分析
链表的基础操作，需要特殊考虑的是，当头节点是符合条件的数据时，需要将头节点后移。所以进阶的思路是设置一个虚拟的头节点，这样就不需要单独处理了。
## 解题代码

1. 虚拟头节点【O(n)】
~~~
public ListNode removeElements(ListNode head, int val) {
    if (head == null) {
        return head;
    }
    // 因为删除可能涉及到头节点，所以设置dummy节点，统一操作
    ListNode dummy = new ListNode(-1, head);
    ListNode pre = dummy;
    ListNode cur = head;
    while (cur != null) {
        if (cur.val == val) {
            pre.next = cur.next;
        } else {
            pre = cur;
        }
        cur = cur.next;
    }
    return dummy.next;
}
~~~

2. 不使用虚拟头节点【O(n)】
~~~
public ListNode removeElements(ListNode head, int val) {
    while (head != null && head.val == val) {
        head = head.next;
    }
    // 已经为null，提前退出
    if (head == null) {
        return head;
    }
    // 已确定当前head.val != val
    ListNode pre = head;
    ListNode cur = head.next;
    while (cur != null) {
        if (cur.val == val) {
            pre.next = cur.next;
        } else {
            pre = cur;
        }
        cur = cur.next;
    }
    return head;
}
~~~