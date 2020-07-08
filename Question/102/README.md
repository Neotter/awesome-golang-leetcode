<!--
 * @Author: Nettor
 * @Date: 2020-06-23 22:27:10
 * @LastEditors: Nettor
 * @LastEditTime: 2020-07-08 11:03:22
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
    // layerNode存放每一层的节点
    layerNode := list.New()
    // tempList保存出队后每一个节点的下一层节点
    tempList := list.New()
    layerNode.PushBack(root)
    for i:=0;layerNode.Len() != 0;i++{
        // t用于保存当前层的节点的*值*,因为res要求的格式是个二维数组
        t := make([]int,0)
        // 每一层所有Node都出队
        for layerNode.Len() != 0{
            // 出队
            fmt.Println(layerNode.Len())
            node := layerNode.Front().Value.(*TreeNode)
            // 保存val到t
            t = append(t,(*node).Val)
            // 当前Node的左右节点入队
            if (*node).Left != nil{
                tempList.PushBack((*node).Left)
            }
            if (*node).Right != nil{
                tempList.PushBack((*node).Right)
            }
            layerNode.Remove(layerNode.Front())
        }
        // 保存t到结果
        res = append(res,t)
        // 连接两个链表
        layerNode.PushBackList(tempList)
        // 清空链表
        tempList.Init()
    }
    return res
}
```
