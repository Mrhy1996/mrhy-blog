---
title: 斐波那契数列
toc: true
date: 2021-04-26 13:17:18
tags: 剑指offer
url: problem-leetcode-jianzhi10-i
---

> 写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：
>
> F(0) = 0,   F(1) = 1
> F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
> 斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> 
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

<!--more-->

思路：如果单纯的斐波那契递归来实现的话，leetcode会提示超时，如下代码

```java
class Solution {
    public int fib(int n) {
            if(n==0){
                return 0;
            }
            if(n==1){
                return 1;
            }
            return fib(n-1)+fib(n-2);
    }
}
```

所以用一个list 数组去存储

```java
class Solution {
    public int fib(int n) {
            if(n==0){
                return 0;
            }
            if(n==1){
                return 1;
            }
            List<Integer>list=new ArrayList<>();
            list.add(0);
            list.add(1);
            for(int i=2;i<=n;i++){
              // 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
                list.add((list.get(i-1)+list.get(i-2))%1000000007);
            }
            return list.get(n);
    }
}
```

