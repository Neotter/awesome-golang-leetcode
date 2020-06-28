<!--
 * @Author: Nettor
 * @Date: 2020-06-27 20:39:33
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-27 21:30:13
 * @Description: file content
-->

# Unique Paths

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)
Above is a 7 x 3 grid. How many possible unique paths are there?

**Example 1:**

```go
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

**Example 2:**

```go
Input: m = 7, n = 3
Output: 28
```

**Constraints:**

- 1 <= m, n <= 100
- It's guaranteed that the answer will be less than or equal to 2 \* 10 ^ 9.

## Solution

动态规划三要素:

- 定义 dp 的含义
- 初始化 dp
- 找到状态转换公式

详细的可以看这篇文章:[告别动态规划，连刷 40 道题，我总结了这些套路，看不懂你打我](https://zhuanlan.zhihu.com/p/91582909)

对于这题来说 `dp[i][j]`的含义表示 robot 走到 i,j 这个格子时可能的路线的数目

由于 robot 走第一行和第一列时有且只有一种走法所以初始化第一行和第一列的值为 1

robot 只能从上往下走和从左往右走,即是说只能从`(i-1, j)`或者`(i, j-1)`走到`(i, j)`,`dp[i-1][j]`和`dp[i][j-1]`分别记录了走到`(i-1, j)`和`(i-1, j)`这两个格子时可能路线数目,所以走到`(i, j)`这个格子时可能的路线数目是走到`(i-1, j)`和`(i-1, j)`的可能的路线相加,因此状态转换公式为:dp[i][j] = dp[i-1][j] + dp[i][j-1]

## Go Solution

```go
func uniquePaths(m int, n int) int {

    //Go的二维数组的初始化有点麻烦
    dp := make([][]int,m)
    for i := 0; i < m; i++{
        dp[i] = make([]int,n)
    }

    //初始化初值
    //第一行
    for i := 0; i < n; i++{
        dp[0][i] = 1
    }
    //第一列
    for i := 1; i < m; i++{
        dp[i][0] = 1
    }

    //状态转换
    for i := 1; i < m; i++{
        for j := 1; j < n; j++{
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
            //dp[j] = dp[j] + dp[j-1]
        }
    }
    return dp[m-1][n-1]
}
```

优化空间复杂度之后

```go
func uniquePaths(m int, n int) int {

    if m <= 0 || n <= 0{
        return 0;
    }

    dp := make([]int,n)

    //初始化初值

    for i := 0; i < n; i++{
        dp[i] = 1
    }

    //状态转换
    for i := 1; i < m; i++{
        dp[0] = 1
        for j := 1; j < n; j++{
            dp[j] = dp[j] + dp[j-1]
        }
    }
    return dp[n-1]
}
```
