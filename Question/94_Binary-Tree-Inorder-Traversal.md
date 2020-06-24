<!--
 * @Author: Nettor
 * @Date: 2020-06-08 17:30:56
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-09 13:16:46
 * @Description: file content
-->

# Binary Tree Inorder Traversal

## Java Solution

```java
/**
* Definition for a binary tree node.
* public class TreeNode {
*     int val;
*     TreeNode left;
*     TreeNode right;
*     TreeNode() {}
*     TreeNode(int val) { this.val = val; }
*     TreeNode(int val, TreeNode left, TreeNode right) {
*         this.val = val;
*         this.left = left;
*         this.right = right;
*     }
* }
*/
//非递归实现中序遍历：中序遍历，左中右
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        //存放结果
        List<Integer> ret = new ArrayList<>();
        //边界条件：如果树为空
        if(root==null) return ret;
        //非递归实现：需要用栈
        Stack<TreeNode> stack = new Stack<>();
        //根节点
        TreeNode cur = root;
        //开始遍历: 当前节点不为空还有栈非空时都会继续遍历
        while (cur != null || !stack.isEmpty()) {
            //因为中序遍历的顺序为左中右：所以先把左节点全部压入栈中
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            //弹出左节点，并添加到结果中
            TreeNode node = stack.pop();
            ret.add(node.val);
            //开始把右节点放入到栈中
            cur = node.right;
        }
        return ret;
    }
}
```

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
