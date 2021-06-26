---
title: 替换空格
toc: true
date: 2021-04-25 23:27:28
tags: 剑指offer
url: problem-leetcode-jianzhi05
---

> 请实现一个函数，把字符串 s 中的每个空格替换成"%20"。
>
>  
>
> 示例 1：
>
> 输入：s = "We are happy."
> 输出："We%20are%20happy."
>
>
> 限制：
>
> 0 <= s 的长度 <= 10000
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

<!--more-->

解题思路：String转换成char数组，遍历

```java
class Solution {
    public String replaceSpace(String s) {
        char[]schar=s.toCharArray();
        StringBuilder sb=new StringBuilder();
        for(char temp:schar){
            if(' '==temp){
                sb.append("%20");
            }else{
                sb.append(temp);
            }
        }
        return sb.toString();
    }
}
```

