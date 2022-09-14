---
title: S1D7|leetcode707|Design Linked List
categories: 算法
tags:
  - leetcode
  - 链表
date: 2022-07-28 11:44:06
description: 每日一题第一赛季第七题
---
## 题目信息
[leetcode 707. Design Linked List](https://leetcode.com/problems/design-linked-list/)
## 题目分析
手写链表的五个基础操作方法，可以更好的了解和使用链表。
整体：第一次写的时候在每个方法都需要单独考虑长度问题，但实际使用全局变量控制就可以。
get：因为使用了虚拟头节点，所以get的时候要获取index+1位置的数据。
## 解题代码

1. 单链表
~~~
class MyLinkedList {

    int size;
    ListNode head;
    
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }
    
    public int get(int index) {
        if(index <0 || index > size-1){
            return -1;
        }
        ListNode cur = head;
        for (int i = 0; i < index+1; i++) {
            cur = cur.next;
        }
        return cur.val;
    }

    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    public void addAtIndex(int index, int val) {
        if(index > size){
            return;
        }
        if(index < 0){
            index  = 0;
        }
        ListNode cur = head;
        ListNode ins = new ListNode(val);
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }
        ins.next = cur.next;
        cur.next = ins;
        size ++;
    }

    public void deleteAtIndex(int index) {
        if (index <0 || index >=size){
            return;
        }
        ListNode cur = head;
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }
        cur.next = cur.next.next;
        size--;
    }
}
~~~

2. 双链表（待补充）
~~~

~~~