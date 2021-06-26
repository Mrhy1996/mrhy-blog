---
title: 二维数组中的查找
toc: true
date: 2021-04-25 23:07:36
tags: 剑指offer
url: problem-leetcode-jianzhi04
---

> 在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
>
>  
>
> 示例:
>
> 现有矩阵 matrix 如下：
>
> [
>   [1,   4,  7, 11, 15],
>   [2,   5,  8, 12, 19],
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],
>   [18, 21, 23, 26, 30]
> ]
> 给定 target = 5，返回 true。
>
> 给定 target = 20，返回 false。
>
>  
>
> 限制：
>
> 0 <= n <= 1000
>
> 0 <= m <= 1000
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

<!--more-->	

思路如下：从矩阵的右上角开始遍历，如果比target小，则col++，如果比target大，则row--，找到target则返回true，找不到则返回false；

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
         if(matrix == null || matrix.length == 0) {
            return false;
        }
      // m 代表行数，n代表列数
        int m = matrix.length, n = matrix[0].length;
        int row = 0, col = n - 1;
        while(row < m && col >= 0) {
            if(matrix[row][col] > target) {
                col--;
            }else if(matrix[row][col] < target) {
                row++;
            }else {
                return true;
            }
        }
        return false;
    }
}
```



