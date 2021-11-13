# 剑指 Offer 27. 二叉树的镜像

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

```
示例 1：
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

## 提示

- 0 <= 节点个数 <= 1000

# 自己的思路

子问题其实就是遍历每个根节点，进行左子树和右子树对换即可。

# 我的代码

```
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func mirrorTree(root *TreeNode) *TreeNode {
    if root == nil{
        return nil
    }
    swap(root)
    mirrorTree(root.Right)
    mirrorTree(root.Left)
    return root
}

func swap(root *TreeNode){
    temp := root.Right
    root.Right = root.Left
    root.Left = temp
}
```

# 更好的思路

## 递归

这是一道很经典的二叉树问题。显然，我们从根节点开始，递归地对树进行遍历，并从叶子节点先开始翻转得到镜像。如果当前遍历到的节点 root 的左右两棵子树都已经翻转得到镜像，那么我们只需要交换两棵子树的位置，即可得到以 root 为根节点的整棵子树的镜像。

# 规范代码

```go
func mirrorTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    left := mirrorTree(root.Left)
    right := mirrorTree(root.Right)
    root.Left = right
    root.Right = left
    return root
}
```

# 思考

这样的递归去考虑思路比较清楚。注意子问题的拆解。