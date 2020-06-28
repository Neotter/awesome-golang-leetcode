<!--
 * @Author: Nettor
 * @Date: 2020-06-28 14:44:37
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-28 15:37:48
 * @Description: file content
-->

# Shuffle an Array

Shuffle a set of numbers without duplicates.

**Example:**

```go
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

## Solution

我觉得这题主要还是考察怎么用 struct 实现一个类型以及其方法, 注意 Shuffle 方法中需要用 `copy()` 复制一份已经随机排序好的数组,不然会冲掉原数组.

## Go Solution

```go
type Solution struct {
    o []int
    c []int
}


func Constructor(nums []int) Solution {
    return Solution{o:nums}
}


/** Resets the array to its original configuration and return it. */
func (this *Solution) Reset() []int {
    return this.o
}


/** Returns a random shuffling of the array. */
func (this *Solution) Shuffle() []int {
    if this.c == nil {
        this.c = make([]int,len(this.o))
        copy(this.c,this.o)
    }
    for i := 1; i < len(this.c); i++{
        r :=rand.Intn(i+1)
        this.c[r], this.c[i] = this.c[i],this.c[r]
    }
    return this.c
}


/**
 * Your Solution object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.Reset();
 * param_2 := obj.Shuffle();
 */
```
