# 剑指 Offer 28. 对称的二叉树

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

```
例 1：
输入：root = [1,2,2,3,4,4,3]
输出：true

示例 2：
输入：root = [1,2,2,null,3,null,3]
输出：false
```

## 提示

- 0 <= 节点个数 <= 1000

# 自己的思路

递归的方法，先判断根节点的左节点和右节点是否相等，如果不相等则返回false，接下来判断左子树和右子树是否对称，判断规则为左子树的左节点必须等于右子树的右节点，左子树的右节点必须等于右子树的左节点，然后递归调用即可。

# 我的代码(unsolved)

```
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSymmetric(root *TreeNode) bool {
    ans := false
    if root.Left != root.Right{
        return false
    }
    ans = helper(root.Left,root.Right)
    return ans

}

func helper(left *TreeNode, right *TreeNode) bool{
    if left.Left != right.Right{
        return false
    }
    if left.Right != right.Left{
        return false
    }
    return true && helper(left.Left,right.Right) && helper(left.Right, right.Left)
}
```

# 更好的思路

## 递归

- 对称二叉树定义： 对于树中 任意两个对称节点 L 和 R ，一定有：
  - L.val = R.val ：即此两对称节点值相等。
  - L.left.val = R.right.val ：即 L 的 左子节点 和 R 的 右子节点 对称；
  - L.right.val = R.left.val ：即 L 的 右子节点 和 R 的 左子节点 对称。

- 根据以上规律，考虑从顶至底递归，判断每对节点是否对称，从而判断树是否为对称二叉树。
- 算法流程
  - isSymmetric(root) ：
    - 特例处理： 若根节点 root 为空，则直接返回 true 
    - 返回值： 即 helper(root.left, root.right) ;
  - helper(L,R)
    - 终止条件
      - 当 L 和 R 同时越过叶节点： 此树从顶至底的节点都对称，因此返回 true ；
      - 当 L 或 R 中只有一个越过叶节点： 此树不对称，因此返回 false ；
      - 当节点 L 值 != 节点 R 值： 此树不对称，因此返回 false ；
    - 递推
      - 判断两节点 L.left和 R.right 是否对称，即 recur(L.left, R.right) ；
        判断两节点 L.right 和 R.left 是否对称，即 recur(L.right, R.left) ；
  - 返回值
    - 两对节点都对称时，才是对称树，因此用与逻辑符 `&&` 连接。

# 规范代码

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSymmetric(root *TreeNode) bool {
    if root == nil{
        return true
    }
    return helper(root.Left,root.Right)
}

func helper(left *TreeNode, right *TreeNode) bool{
    if left == nil && right == nil{ return true }
    if left == nil || right == nil || left.Val != right.Val {
        return false
    }
    return helper(left.Left,right.Right) && helper(left.Right, right.Left)
}
```

# 思考

其实自己差一点就写出来了，但是对nil的处理并不聪明，卡在了这里。