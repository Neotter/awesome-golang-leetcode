<!--
 * @Author: Nettor
 * @Date: 2020-07-08 14:15:49
 * @LastEditors: Nettor
 * @LastEditTime: 2020-07-08 14:21:35
 * @Description: file content
-->

# Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```go
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```go
[
  [3],
  [20,9],
  [15,7]
]
```

## Solution

和 102 按层遍历差不多

只需要加多一个`var order bool`变量控制入栈的顺序就行了

这样每次遍历一层的时候必然是下一层顺序的头部在栈底,因此顶部出栈的时候必然是在逆序的.

## Go Solution

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func zigzagLevelOrder(root *TreeNode) [][]int {
        res := make([][]int,0)
    if root == nil{
        return res
    }
    preLayer := list.New()
    nextLayer := list.New()
    preLayer.PushBack(root)
    //order 为true是从左到右,为false是从右到左
    order := true
    for i:=0; preLayer.Len() != 0; i++{
        // valArray用于每层出队的节点的值
        valArray := make([]int,0)
        for preLayer.Len() != 0{
            node := preLayer.Back().Value.(*TreeNode)
            valArray = append(valArray,(*node).Val)
            if order == true{
                if (*node).Left != nil{
                    nextLayer.PushBack((*node).Left)
                }
                if (*node).Right != nil{
                    nextLayer.PushBack((*node).Right)
                }
            } else {
                if (*node).Right != nil{
                    nextLayer.PushBack((*node).Right)
                }
                if (*node).Left != nil{
                    nextLayer.PushBack((*node).Left)
                }
            }
            preLayer.Remove(preLayer.Back())

        }
        // 顺序改变
        order = !order
        // 加入这层的结果
        res = append(res,valArray)
        // 把下一层的链表移动到上一层
        preLayer.PushBackList(nextLayer)
        // 清空上一层
        nextLayer.Init()
    }
    return res
}
```
