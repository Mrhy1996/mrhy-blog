---
title: 用两个栈实现队列
toc: true
date: 2021-04-26 11:28:17
tags: 剑指offer
url: problem-leetcode-jianzhi06
---

> 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )
>
>  
>
> 示例 1：
>
> 输入：
> ["CQueue","appendTail","deleteHead","deleteHead"]
> [[],[3],[],[]]
> 输出：[null,null,3,-1]
> 示例 2：
>
> 输入：
> ["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
> [[],[],[5],[2],[],[]]
> 输出：[null,-1,null,null,5,2]
> 提示：
>
> 1 <= values <= 10000
> 最多会对 appendTail、deleteHead 进行 10000 次调用
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

<!--more-->

思路：两个栈实现队列，一个栈（first）负责入，一个栈（second）负责出，当出栈（second）为空时，把入栈（first）的元素放入出栈（second），再进行出栈。

```java
class CQueue {

    Stack<Integer>firstStack;
    Stack<Integer>secondStack;

    public CQueue() {
        firstStack=new Stack<>();
        secondStack=new Stack<>();
    }
    
    public void appendTail(int value) {
        firstStack.push(value);
    }
    public int deleteHead() {
      // 如果出栈为空，则看栈1的情况
        if(secondStack.isEmpty()){
          // 如果栈1也为空，则说明两个栈都没有元素，说明队列为空，返回-1；
            if(firstStack.isEmpty()){
                return -1;
            }else{
              // 遍历栈1，将栈1的元素放入栈2;
                while(!firstStack.isEmpty()){
                    secondStack.push( firstStack.pop());
                }
            return secondStack.pop();
            }
        }else{
            return secondStack.pop();
        }

    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```



