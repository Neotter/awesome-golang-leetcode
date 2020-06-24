<!--
 * @Author: Nettor
 * @Date: 2020-06-08 17:30:56
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-24 13:10:51
 * @Description: file content
-->

# Binary Tree Inorder Traversal

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


func inorderTraversal(root *TreeNode) []int {
    ret := make([]int,0)
    stack := make([]*TreeNode,0)

    if root == nil {
        return ret
    }

    curNode := root
    for curNode != nil || len(stack) > 0 {
        for curNode != nil {
            stack = append(stack, curNode)
            curNode = curNode.Left
        }

        top := stack[len(stack) -1]
        stack = stack[0:len(stack) -1]
        ret = append(ret, top.Val)
        curNode = top.Right

    }
    return ret
}
```

涉及到的 go 语言知识：For, make(), append()
