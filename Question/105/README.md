<!--
 * @Author: Nettor
 * @Date: 2020-06-30 15:28:52
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-30 16:18:39
 * @Description: file content
--> 

# Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```go
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```go
    3
   / \
  9  20
    /  \
   15   7
```

# Solution

例子:

```go
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

先序遍历:根->左子树->右子树
中序遍历:左子树->右子树->根节点

先序遍历的特点:第一个节点一定是根节点,比如在这个例子的中`3`是整个树的根节点.

中序遍历的特点:无论是对于整棵树的根节点来说还是左右子树的根节点来说,一个节点的左边的所有的值一定是其左子树,右边的所有的值一定是其右子树. 比如`[9,3,15,20,7]`中,`3`的左边的值`9`一定是左子树的所有的值,右边`15,20,7`一定是右子树的所有的值.

这下我们从中序遍历中知道了左右子树的长度,回到先序遍历,根据左右子树的长度和`根->左子树->右子树`的规则,我们又知道了在先序遍历中,`9`是左子树,`20,15,7`是右子树.

最后把`9`和`20,15,7`当成独立的树再重复进行以上的工作,就可以重构出整颗树了.

# Go Solution

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 || len(inorder) ==0 {
        return nil
    }

    root := new(TreeNode)
    root.Val = preorder[0]
    index := 0
    for i,item := range inorder{
        if item == preorder[0]{
            index = i
            break
        }
    }
    root.Left = buildTree(preorder[1:index+1],inorder[:index])
    root.Right = buildTree(preorder[index+1:],inorder[index+1:])
    
    return root
}
```