# 剑指 Offer 26. 树的子结构

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即A中有出现和B相同的结构和节点值。

```
示例 1：
输入：A = [1,2,3], B = [3,1]
输出：false

示例 2：
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```

## 提示

- 0 <= 节点个数 <= 10000

# 自己的思路

以某一个遍历去遍历两个树，获得两个树的所有数据集合，然后判断子树集合是否是树的子集合。

啊这，结果方法没有通过第47个测试用例，即[1,0,1,-4,-3] 和 [1,-4]。

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


func isSubStructure(A *TreeNode, B *TreeNode) bool {
    var aNodes []int
    var bNodes []int
    if B == nil{
        return false
    }
    getElements(A,&aNodes)
    getElements(B,&bNodes)
    i, j := 0, 0
    for i < len(aNodes) && j < len(bNodes){
        if aNodes[i] != bNodes[j]{
            i++
        } else{
            i++
            j++
        }
    }
    if j == len(bNodes){
        return true
    }
    return false
}

func getElements(a *TreeNode,nodes *[]int){
    if a == nil{
        return 
    }
    *nodes = append(*nodes,a.Val)
    getElements(a.Left,nodes)
    getElements(a.Right,nodes)
}
```

# 更好的思路

## 方法一：队列辅助

首先通过创建一个节点队列按照层次遍历遍历A树节点，每次取出一个值与B.Val进行比较，如果相同则观察该节点的子树与B子树是否相容，即B子树属于该节点子树的一部分。

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


/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSubStructure(A *TreeNode, B *TreeNode) bool {
    // 根据题目判断条件，初始有空节点返回false
    if A == nil || B == nil{
        return false
    }
    queue, root := []*TreeNode{A}, A
    for len(queue) > 0{
        root = queue[0]
        queue = queue[1:]
        if root.Val == B.Val && helper(root, B){
            return true
        }
        // 还没遍历完，而且没有找到匹配的子树，继续遍历A子树
        if root.Left != nil {
            queue = append(queue, root.Left)
        }
        if root.Right != nil {
            queue = append(queue, root.Right)
        }
    }
    return false
}

func helper (A *TreeNode, B *TreeNode) bool {
    // B为空依然返回True，因为B的子树可能是空的，但是A为空就不行了
    if B == nil {
        return true
    }
    if A == nil {
        return false
    }
    if A.Val == B.Val {
        return helper(A.Left, B.Left) &&  helper(A.Right, B.Right)  //递归
    }else{
        return false
    }
}


// 方法二的代码
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */


/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSubStructure(A *TreeNode, B *TreeNode) bool {
    if B == nil || A == nil{
        return false
    }
    // 遍历A中每个节点，A树中任一节点包含B就能返回true
    return helper(A,B) || isSubStructure(A.Left,B) || isSubStructure(A.Right,B)
}

// 包含：以A为根的子树是否包含B（必须从A开始）
func helper (A *TreeNode, B *TreeNode) bool {
    if B == nil{
        return true
    }
    if A == nil || A.Val != B.Val{
        return false
    }
    return helper(A.Left,B.Left) && helper(A.Right,B.Right)
}
```

# 思考

这道题也值得多思考