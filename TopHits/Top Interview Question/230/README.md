<!--
 * @Author: Nettor
 * @Date: 2020-06-20 19:01:38
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-20 19:04:06
 * @Description: file content
-->

# Kth Smallest Element in a BST

## Go

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func kthSmallest(root *TreeNode, k int) int {
    cur := root
    top := root
    stack := make([]*TreeNode,0)

    for i:=0; i<k; i++{
        for cur != nil{
            stack = append(stack,cur)
            cur = cur.Left
        }
        top = stack[len(stack)-1]
        stack = stack[0:len(stack)-1]
        cur = top.Right
    }
    return top.Val
}
```

和第 94 题差不多,中序遍历.
