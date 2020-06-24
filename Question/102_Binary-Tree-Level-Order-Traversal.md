<!--
 * @Author: Nettor
 * @Date: 2020-06-23 22:27:10
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-23 22:27:39
 * @Description: file content
--> 

# Binary Tree Level Order Traversal

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
func levelOrder(root *TreeNode) [][]int {
    res := make([][]int,0)
    if root == nil{
        return res
    }
    listA := list.New()
    tempList := list.New()
    listA.PushBack(root)
    for i:=0;listA.Len() != 0;i++{
        t := make([]int,0)
        for listA.Len() != 0{
            fmt.Println(listA.Len())
            node := listA.Front().Value.(*TreeNode)
            t = append(t,(*node).Val)
            if (*node).Left != nil{
                tempList.PushBack((*node).Left)
            }
            if (*node).Right != nil{
                tempList.PushBack((*node).Right)
            }
            listA.Remove(listA.Front())
        }
        res = append(res,t)
        listA.PushBackList(tempList)
        tempList.Init()
    }
    return res
}
```