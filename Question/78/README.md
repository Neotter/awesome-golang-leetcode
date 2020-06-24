<!--
 * @Author: Nettor
 * @Date: 2020-06-19 15:20:30
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-19 15:26:10
 * @Description: file content
-->

# Subsets

## Go Solution

```go
func subsets(nums []int) [][]int {
    res := make([][]int,0)
    res = append(res,make([]int,0))
    sort.Ints(nums)
    for _,num := range nums{
        for _,resElement := range res{
            t := make([]int,len(resElement))
            copy(t,resElement)
            t = append(t,num)
            res = append(res,t)
        }
    }
    return res
}
```

这里我忽略了,在 Go 中 for range 循环,range 前如果只有一个临时变量必然会返回数组的 index,在 range 前如果有两个临时变量,前一个是 index,后一个是 value.
还有使用 slice 的情况下用 copy 复制一份应该是常规操作了
