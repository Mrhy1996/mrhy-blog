---
title: 青蛙跳台阶问题
toc: true
date: 2021-04-26 14:09:45
tags: 剑指offer
url: problem-leetcode-jianzhi10-ii
---

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> 示例 1：
>
> 输入：n = 2
> 输出：2
> 示例 2：
>
> 输入：n = 7
> 输出：21
> 示例 3：
>
> 输入：n = 0
> 输出：1
> 提示：
>
> 0 <= n <= 100
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

<!--more-->

思路：这是典型的斐波那契数列，分析一下，青蛙跳上n级台阶的跳法，比如跳3级的时候有3种，调4级的时候，F（3）+F（2），这个结论思想是：青蛙可以留下1级台阶，也可以留下2级台阶，所以F(n)=F(n-1)+F(n-2)

```java
class Solution {
    public int numWays(int n) {
          if(n==0){
                return 1;
            }
            if(n==1){
                return 1;
            }
            List<Integer>list=new ArrayList<>();
            list.add(1);
            list.add(1);
            for(int i=2;i<=n;i++){
              // 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
                list.add((list.get(i-1)+list.get(i-2))%1000000007);
            }
            return list.get(n);
    }
}
```

