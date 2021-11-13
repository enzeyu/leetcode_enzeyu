# 剑指 Offer 7. 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请构建该二叉树并返回其根节点。

假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

```
 示例 1:
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]

示例 2:
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

## 提示

- 0 <= 节点个数 <= 5000

# 自己的思路

注意前序遍历的第一个节点就是中间节点，故找到的树根节点就是preorder[0]，然后将左子树和右子树的前序中序数字找到，重新放到buildTree里进行递归。

# 我的代码

```
func buildTree(preorder []int, inorder []int) *TreeNode {
    // 递归退出条件
    if len(preorder) == 0 {
        return nil
    }
    root := &TreeNode{preorder[0], nil, nil}
    i := 0
    // 在中序遍历里找到root节点
    for ; i < len(inorder); i++ {
        if inorder[i] == preorder[0] {
            break
        }
    }
    // 将左子树元素和右子树元素分别递归
    root.Left = buildTree(preorder[1:len(inorder[:i])+1], inorder[:i])
    root.Right = buildTree(preorder[len(inorder[:i])+1:], inorder[i+1:])
    return root
}
```

# 更好的思路

# 规范代码

```go

```

# 思考

这题以前做过了，注意寻找到根节点很重要。