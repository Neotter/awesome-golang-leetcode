<!--
 * @Author: Nettor
 * @Date: 2020-06-28 16:57:12
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-28 17:09:30
 * @Description: file content
-->

# Flatten Nested List Iterator

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Example 1:**

```go
Input: [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false,
             the order of elements returned by next should be: [1,1,2,1,1].
```

**Example 2:**

```go
Input: [1,[4,[6]]]
Output: [1,4,6]
Explanation: By calling next repeatedly until hasNext returns false,
             the order of elements returned by next should be: [1,4,6].
```

## Solution

这题和 384 其实都是设计类的方法的题目,没有太多好讲的

struct 里面有一个[]int 保存已经 flatten 的结果，再用一个 int 保存上一个 next 的索引。

也可以用一个二维数组直接保存 List 的 nestedList 已经 next 过的索引，用节省空间复杂度，时间换空间。

## Go Solution

```go
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * type NestedInteger struct {
 * }
 *
 * // Return true if this NestedInteger holds a single integer, rather than a nested list.
 * func (this NestedInteger) IsInteger() bool {}
 *
 * // Return the single integer that this NestedInteger holds, if it holds a single integer
 * // The rult is undefined if this NestedInteger holds a nested list
 * // So before calling this method, you should have a check
 * func (this NestedInteger) GetInteger() int {}
 *
 * // Set this NestedInteger to hold a single integer.
 * func (n *NestedInteger) SetInteger(value int) {}
 *
 * // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 * func (this *NestedInteger) Add(elem NestedInteger) {}
 *
 * // Return the nested list that this NestedInteger holds, if it holds a nested list
 * // The list length is zero if this NestedInteger holds a single integer
 * // You can access NestedInteger's List element directly if you want to modify it
 * func (this NestedInteger) GetList() []*NestedInteger {}
 */

type NestedIterator struct {
    r []int
    hi int
}

func Constructor(nestedList []*NestedInteger) *NestedIterator {
    //注意使用new()返回的结构体的指针所以可以直接return
    //iter:=NestedIterator{}返回的是结构体的值，不能直接return
    iter := new(NestedIterator)
    flatten(nestedList,iter)
    return iter
}

func flatten(l []*NestedInteger, i *NestedIterator){
    for _,item := range l{
        if item.IsInteger() {
            i.r = append(i.r,item.GetInteger())
        } else {
            flatten(item.GetList(),i)
        }
    }
}

func (this *NestedIterator) Next() int {
    item := this.r[this.hi]
    this.hi++
    return item
}

func (this *NestedIterator) HasNext() bool {
    return this.hi < len(this.r)
}
```
