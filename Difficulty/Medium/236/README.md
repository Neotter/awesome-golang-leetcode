<!--
 * @Author: Nettor
 * @Date: 2020-07-15 13:41:44
 * @LastEditors: Nettor
 * @LastEditTime: 2020-07-15 14:30:35
 * @Description: file content
-->

# Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a _descendant of itself_).”

Given the following binary tree: root = [3,5,1,6,2,0,8,null,null,7,4]

！[](./img/binarytree.png)

**Example 1:**

```go
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

```go
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Note:**

- All of the nodes' values will be unique.
- p and q are different and both values will exist in the binary tree.

## Solution

### 递归判断

解这题需要考虑遍历到达一个节点时应该有那些可能的情况，并且应该怎么处理这些可能的情况。

那么首先是可能的情况：

可能的情况：

- 当前节点是其中之一，直接返回；
- 当前节点不是其中之一；
  - 目标节点分散在当前节点的左右子树中；
  - 目标节点集中在当前节点的其中一个子树中；

根据以上的逻辑设计递归：

- 当前节点是目标节点之一，返回；
- 当前节点不是目标节点之一，但是在左右子树的最终递归中找到了目标节点且分散在左右子树中（最终返回都非 null），则当前节点是共同祖节点；
- 当前节点不是目标节点之一，且左右子树递归中有一方返回了 null，则非 null 的一方继续前两步的递归逻辑

## Go Solution

```go
/**
 * Definition for TreeNode.
 * type TreeNode struct {
 *     Val int
 *     Left *ListNode
 *     Right *ListNode
 * }
 */
 func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {

     // 先想结束条件是什么

     if root == nil{
         return nil
     }

     if p == root || q == root{
         return root
     }

     // 再想这个函数到到底是干什么用的
     // 这个函数是为了找出最接近的公共的父类
     // 公共父类的特点是左边和右边都存在一个target
     // 如果没有那就返回nil
     l := lowestCommonAncestor(root.Left,p,q)
     r := lowestCommonAncestor(root.Right,p,q)

     // 有多种情况
     // 第一种:左右都存在,那么当前的就是公共父类
     if l != nil && r != nil{
         return root
     // 如果当前节点都不存在target,那么他不是公共父类
     } else if l == nil && r == nil{
         return nil
     } else{
         // 如果有一方存在那么当前节点只是某一方的父类,公共父类还在上一层,返回告诉他们这个节点只存在一个
         if l == nil{
             return r
         } else{
             return l
         }
     }
     return root
}
```
