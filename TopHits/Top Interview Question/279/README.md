<!--
 * @Author: Nettor
 * @Date: 2020-07-01 16:24:36
 * @LastEditors: Nettor
 * @LastEditTime: 2020-07-01 16:37:53
 * @Description: file content
-->

# Perfect Squares

Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

**Example 1:**

```go
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
```

**Example 2:**

```go
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

## Solution

四平方和定理:

- 所有的正整数都可以表示为 4 个整数的平方和
- 数字 4a 和数字 a 所需要的平方和个数相同,并且如果 n%8=7 那么 n 肯定需要 4 个平方数构成
- 0 也是整数

因此组成 n 的最小平方和的数最多只有 4 个,有 4 种情况:

- 如果 n%8=7 那么 n 肯定需要 4 个平方数构成
- 如果 n != a^2 + b^2,那么 n 肯定需要 3 个平方数构成
- 如果 n == a^2 + b^2,a 和 b 都为 0,那么 n 肯定需要 2 个平方数构成
- 如果 n == a^2 + b^2,a 或者 b 其中之一 0,那么 n 肯定需要 1 个平方数构成

只要穷举 a 和 b 就行了。

## Go Solution

```go
func numSquares(n int) int {
    // 所有的正整数都可以表示为4个整数的平方和
    // 4a和a需要的完全平方数个数相同，所以一直除4直到不能被4整除
    for n%4 == 0{
        n /= 4
    }
    // 如果num%8=7，则num需要四个平方数构成
    if n%8 == 7{
        return 4
    }
    // 从0开始穷举所有数做平方，如果有两个数的平方相加能够等于n，其中一个数不等于0，那就答案就是1，如果两个数都不等于0，那答案就是1，穷举玩都不是答案，那答案就是3
    for a := 0; a * a <=n; a++ {
        b := int(math.Sqrt(float64(n-a*a)))
        fmt.Println(b)
        if a * a + b * b == n{
            if a > 0 && b > 0{
                return 2
            }
            if a > 0 || b > 0{
                return 1
            }
        }
    }
    return 3;

}
```
