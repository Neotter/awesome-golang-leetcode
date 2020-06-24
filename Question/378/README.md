<!--
 * @Author: Nettor
<<<<<<< HEAD
 * @Date: 2020-06-23 22:16:00
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-23 22:25:11
=======
 * @Date: 2020-06-23 16:36:00
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-23 16:44:10
>>>>>>> c557db206fe7793c2e308e52c23686634643b795
 * @Description: file content
--> 

# Kth Smallest Element in a Sorted Matrix

## Go Solution

```go
func kthSmallest(matrix [][]int, k int) int {
    if len(matrix) == 0 {
        return 0
    }
    m := len(matrix)
    low, high := matrix[0][0], matrix[m-1][m-1]
<<<<<<< HEAD
    //二分查找
    for low < high {
        //根据最大值最小值找到中值
=======
    for low < high {
>>>>>>> c557db206fe7793c2e308e52c23686634643b795
        mid := (low + high) / 2
        cc := helper(matrix, mid)
        if cc < k {
            low = mid + 1
        } else {
            high = mid
        }
    }
    return low
}

func helper(matrix [][]int, target int) int{
    c := 0
<<<<<<< HEAD
    //从左下角开始扫描,扫描到右上角
=======
>>>>>>> c557db206fe7793c2e308e52c23686634643b795
    i, j := len(matrix)-1, 0
    for i >= 0 && j <= len(matrix)-1 {
        if matrix[i][j] <= target {
            j++
            c += i + 1
        } else {
            i--
        }
    }
    return c
}
```