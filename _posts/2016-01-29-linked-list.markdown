---
layout: post
title: Classical Programming Problem - Linked List / 经典编程问题 - 链表
date: 2016-01-29 22:58
categories: [blog ]
tags: [Algorithm, ]
description:
---

Baisc Singly Linked List / 基本的单链表

```
public class ListNode {
```
int val;
ListNode next = null;
```

```

```

```
ListNode(int val) {
```
this.val = val;
```
}
```
}
```



## Q1 - Remove a Single Node

Implement an algorithm to delete a node in the middle of a singly linked list, give only access to that node.
if this node is the last node, return `false`, otherwise return `true`.

实现一个算法，删除单向链表中间的某个结点，假定你只能访问该结点。
给定带删除的节点，请执行删除操作，若该节点为尾节点，返回`false`，否则返回`true`。

Solution:

```
public class Remove {
```
public boolean removeNode(ListNode pNode) {
```
// write code here
if(pNode == null || pNode.next == null)
    return false;
pNode.val = pNode.next.val;
pNode.next = pNode.next.next;
return true;
```
}
```
}
```

P.S. Change your mind, modify the value rather than pointer.



## Q2 - Add Two Numbers by a Linked List

You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list. EXAMPLE: Input: (7->1->6),(5->9->2) Output: 2->1->9

有两个用链表表示的整数，每个结点包含一个数位。这些数位是反向存放的，也就是个位排在链表的首部。编写函数对这两个整数求和，并用链表形式返回结果。
给定两个链表`ListNode* A`，`ListNode* B`，请返回A+B的结果`(ListNode*)`。
测试样例：输入: (7->1->6),(5->9->2) 输出: 2->1->9

Solution:

```
public class Plus {
```
public ListNode plusAB(ListNode a, ListNode b) {
```
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
```
```

```

```

```
```
if(a==null&&b!=null)
    b=b.next;
else if(a!=null&&b==null)
    a=a.next;
else{
    a=a.next;
    b=b.next;
}
```
```

```

```

```
```
}
if(plus==1){
    current.next = new ListNode(plus);
}
return start;
```
}
```
}
```




## Q3 - Linked List Partition

Question is: Write code to partition a linked list around a value `x`, such that all nodes less than `x` come before all nodes greater than or equal to `x`.

编写代码，以给定值`x`为基准将链表分割成两部分，所有小于`x`的结点排在大于或等于`x`的结点之前给定一个链表的头指针`ListNode* pHead`，请返回重新排列后的链表的头指针。注意：分割以后保持原来的数据顺序不变。

Solution:

```
public class Partition {
```
public ListNode partition(ListNode pHead, int x) {
```
if(pHead == null || pHead.next == null)
    return pHead;
```
```

```

```

```
```
ListNode moveBeforeX=new ListNode(0);
ListNode moveAfterX=new ListNode(0);
ListNode beforeX=moveBeforeX;
ListNode afterX=moveAfterX;
ListNode temp = pHead;
```
```

```

```

```
```
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
```
```

```

```

```
```
moveBeforeX.next=afterX.next;
moveAfterX.next=null;
return beforeX.next;
```
```

```

```

```
}
```
}
```


## Q4 - Reversed a Linked List

Reversed a Linked List.
转置一个链表。

Solution 1:

```
ListNode reverseList(ListNode pHead)  
{  
```

```

```
if ( (null == pHead) || (null == pHead.next) )
```
return pHead;  
```
```

```

```

```
ListNode pNewHead = reverseList(pHead.next);  
pHead.next.next = pHead;  
pHead.next = null;  
```

```

```

```
return pNewHead;  
```
}
```

Solution 2:

```
public class Solution {
```
public ListNode ReverseList(ListNode head) {
(head == null || head.next == null)
```
    return head;
ListNode p1 = head;
ListNode p2 = p1.next;
ListNode p3 = p2.next;
p1.next = null;
```
```

```

```

```
```
while(p3!=null){
    p2.next = p1;
    p1 = p2;
    p2 = p3;
    p3 = p2.next;            
}
p2.next = p1;
head = p2;
```
```

```

```

```
```
return head;
```
   }
```
}
```


## Q5 - Check Palindrome

Implement a function to check if a linked list is a palindrome.

请编写一个函数，检查链表是否为回文。
给定一个链表`ListNode* pHead`，请返回一个`bool`，代表链表是否为回文。

Solution 1:

Reserve the linked list and compare each node value (actually half numbers is enough).

Solution 2:

```
public class Palindrome {
```
public boolean isPalindrome(ListNode pHead) {
```
ListNode fast = pHead;
ListNode slow = pHead;
Stack<Integer> stack = new Stack<Integer>();
```
```

```

```

```
```
while(fast != null && fast.next != null){
    stack.push(slow.val);
    slow = slow.next;
    fast = fast.next.next;
}
```
```

```

```

```
```
if (fast != null) {
    slow = slow.next;
}
while(slow != null){
    if (stack.pop() != slow.val) {
        return false;
    }
    slow = slow.next;
}
return true;
```
}
```

```

```
}
```

P.S. Fast/slow pointer strategy is very useful.

## Q6 - Check Loop

Implement a function to check if a linked list has a loop.

输入一个单向链表，判断链表是否有环？

分析：通过两个指针，分别从链表的头节点出发，一个每次向后移动一步，另一个移动两步，两个指针移动速度不一样，如果存在环，那么两个指针一定会在环里相遇。

```
//判断单链表是否存在环,参数circleNode是环内节点，后面的题目会用到
bool hasCircle(Node *head,Node *&circleNode)
{
```
Node *slow,*fast;
slow = fast = head;
while(fast != NULL && fast->next != NULL)
{
```
fast = fast->next->next;
slow = slow->next;
if(fast == slow)
{
    circleNode = fast;
    return true;
}
```
}
```

```

```

```
return false;
```
}
```
