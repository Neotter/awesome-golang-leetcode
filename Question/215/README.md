<!--
 * @Author: Nettor
 * @Date: 2020-06-22 19:29:37
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-22 19:30:19
 * @Description: file content
-->

# Kth Largest Element in an Array

## Go Solution

```go
func findKthLargest(nums []int, k int) int {
    sort.Ints(nums)
    return nums[len(nums)-k]
}
```
