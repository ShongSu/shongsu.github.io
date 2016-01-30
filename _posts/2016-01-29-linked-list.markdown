---
layout: post
title: Classical Interview Problem - Linked List / 经典面试问题 - 链表 
date: 2016-01-29 22:58
post-link:
---

Baisc Singly Linked List / 基本的单链表

    public class ListNode {
        int val;
        ListNode next = null;

        ListNode(int val) {
            this.val = val;
        }
    }



Q1 - Remove a Single Node

Implement an algorithm to delete a node in the middle of a singly linked list, give only access to that node.
if this node is the last node, return false, otherwise return true.

实现一个算法，删除单向链表中间的某个结点，假定你只能访问该结点。
给定带删除的节点，请执行删除操作，若该节点为尾节点，返回false，否则返回true

Solution:

    public class Remove {
        public boolean removeNode(ListNode pNode) {
            // write code here
            if(pNode == null || pNode.next == null)
                return false;
            pNode.val = pNode.next.val;
            pNode.next = pNode.next.next;
            return true;
        }
    }


Q2 - Add Two Numbers by a Linked List 

You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the um as a linked list. EXAMPLE: Input: (7->1->6),(5->9->2) Output: 2->1->9

有两个用链表表示的整数，每个结点包含一个数位。这些数位是反向存放的，也就是个位排在链表的首部。编写函数对这两个整数求和，并用链表形式返回结果。
给定两个链表ListNode* A，ListNode* B，请返回A+B的结果(ListNode*)。
测试样例：输入: (7->1->6),(5->9->2) 输出: 2->1->9

Solution:

    public class Plus {
        public ListNode plusAB(ListNode a, ListNode b) {
            // write code here
            ListNode start = null;
            ListNode current = null;
            if(a==null && b==null)
                return null;
            int plus=0;
            while(a!=null||b!=null){
                int temp = plus;
                if(a!=null)
                    temp += a.val;
                if(b!=null)
                    temp += b.val;
                if(start == null){
                    current = new ListNode(temp%10);
                    start = current;
                    plus = temp/10;
                }else {
                    current.next = new ListNode(temp%10);
                    current=current.next;
                    plus = temp/10;
                }
                
                if(a==null&&b!=null)
                    b=b.next;
                else if(a!=null&&b==null)
                    a=a.next;
                else{
                    a=a.next;
                    b=b.next;
                }
                    
            }
            if(plus==1){
                current.next = new ListNode(plus);
            }
            return start;
        }
    }


Q3 - Linked List Partition

Question is: Write code to partition a linked list around a value x, such that all nodes less than x come before all nodes greater than or equal to x.

编写代码，以给定值x为基准将链表分割成两部分，所有小于x的结点排在大于或等于x的结点之前给定一个链表的头指针 ListNode* pHead，请返回重新排列后的链表的头指针。注意：分割以后保持原来的数据顺序不变。

Solution:

    public class Partition {
        public ListNode partition(ListNode pHead, int x) {
            if(pHead == null || pHead.next == null)
                return pHead;
            
            ListNode moveBeforeX=new ListNode(0);
            ListNode moveAfterX=new ListNode(0);
            ListNode beforeX=moveBeforeX;
            ListNode afterX=moveAfterX;
            ListNode temp = pHead;

            while(temp!=null){  
                if(temp.val<x){
                    moveBeforeX.next=temp;
                    moveBeforeX=temp;
                }else{
                    moveAfterX.next=temp;
                    moveAfterX=temp;
                }
                temp = temp.next;
            }
            
            moveBeforeX.next=afterX.next;
            moveAfterX.next=null;
            return beforeX.next;

        }
    }
    





