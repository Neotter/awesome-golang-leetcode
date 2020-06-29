<!--
 * @Author: Nettor
 * @Date: 2020-06-28 19:16:09
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-28 19:43:25
 * @Description: file content
-->

# Container With Most Water

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and n is at least 2.

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array `[1,8,6,2,5,4,8,3,7]`. In this case, the max area of water (blue section) the container can contain is 49.

**Example:**

```go
Input: [1,8,6,2,5,4,8,3,7]
```

## Solution

数组的特点就是可以随机读取，很快地获得尾部和首部的元素，所以我们可以同时从数组左右两头开始往中间走。由于面积等于`width*Min(height[R],height[L])`，而每走一步 width 必然会减一，所以循环迭代 width 到 0 就可以遍历所有的走法。

剩下的问题是让左边的指针往中间走还是右边的指针往中间边走，那肯定是保留最大的 height 啦，所以让 height 小的一边往中间走。

## Go Solution

```go
func maxArea(height []int) int {
    L,R,width,res := 0,len(height)-1,len(height)-1,0
    for w := width; w > 0 ; w--{
        if height[L] < height[R]{
            res, L = Max(res,w * height[L]), L + 1
        } else {
            res, R = Max(res,w * height[R]), R - 1
        }
    }
    return res
}

// Golang没有针对int型的Max函数我也很绝望呀
// Max returns the larger of x or y.
func Max(x, y int) int {
    if x < y {
        return y
    }
    return x
}
```
