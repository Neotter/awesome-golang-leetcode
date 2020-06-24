<!--
 * @Author: Nettor
 * @Date: 2020-06-20 16:29:42
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-20 16:30:31
 * @Description: file content
-->

# Product of Array Except Self

## Go Solution

```go
func productExceptSelf(nums []int) []int {
    res := make([]int,len(nums))
    temp:=1
    for i:=0; i<len(nums); i++{
        res[i] = temp
        temp *= nums[i]
    }
    temp=1
    for i:=len(nums)-1; i >=0; i--{
        res[i] *= temp
        temp *= nums[i]
    }
    return res
}
```
