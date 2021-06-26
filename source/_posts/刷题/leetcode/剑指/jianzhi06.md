---
title: 从尾到头打印单链表
toc: true
date: 2021-04-25 23:55:24
tags: 剑指offer
url: problem-leetcode-jianzhi06
---

> 输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
>
>  
>
> **示例 1：**
>
> 输入：head = [1,3,2]
> 输出：[2,3,1]

<!--more-->

解题思路：栈是先入后出的，用栈暂存单链表节点的值

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] reversePrint(ListNode head) {
        if(head==null){
            return new int[]{};
        }
        Stack<Integer>stack=new Stack<>();
        ListNode node=head;
        int length=0;
        while(node !=null){
            stack.push(node.val);
            node=node.next;
            length++;
        }
        int[]a=new int[length];
        for(int i=0;i<length;i++){
            a[i]=stack.pop();
        }
        return a;

    }
}
```

另：用栈的地方就可以用递归，可以思考一下递归的写法。