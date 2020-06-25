<!--
 * @Author: Nettor
 * @Date: 2020-06-25 15:40:15
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-25 16:21:45
 * @Description: file content
-->

# 4Sum II

Given four lists A, B, C, D of integer values, compute how many tuples `(i, j, k, l)` there are such that `A[i] + B[j] + C[k] + D[l]` is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -2^28 to 2^28 - 1 and the result is guaranteed to be at most 2^31 - 1.

**Example:**

```go
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

## Solution

这种都是一个套路——hashmap。

先遍历 A 和 B，让 A 和 B 的 Element 两两相加，结果作为 key 保存在一个 hashmap 中，让 key 对应的 value 都为 1。

再遍历 C 和 D，让 C 和 D 的 Element 两两相加，让结果的负数作为 key， 在 hashmap 查找是否存在这个 key 对应的 value。

前后两个遍历时间复杂度都为 O(n^2)。

```go
func fourSumCount(A []int, B []int, C []int, D []int) int {
    res := 0
    unorder_map := make(map[int]int,0)
    for _,i := range A{
        for _,j := range B{
            unorder_map[i+j] += 1
        }
    }

    for _,i := range C{
        for _,j := range D {
            res += unorder_map[-(i+j)]
        }
    }
    return res
}
```
