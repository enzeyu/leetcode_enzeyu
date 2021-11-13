# 剑指 Offer 32.1 从上到下打印二叉树

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

```
例如:
给定二叉树: [3,9,20,null,null,15,7],
返回：[3,9,20,15,7]
```

## 提示

- `节点总数 <= 1000`

# 自己的思路

这道题是广度优先搜索，需要使用队列的数据结构来处理。

# 我的代码(缺少判断)

```
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func levelOrder(root *TreeNode) []int {
    ans := []int{}
    if root == nil{
        return nil
    }
    queue := []*TreeNode{ root }
    for len(queue) > 0{
        ele :=  queue[0]
        queue = queue[1:]
        ans = append(ans,ele.Val)
        queue = append(queue,ele.Left)
        queue = append(queue,ele.Right)
    }
    return ans
}
```

# 更好的思路

自己想的没错，但是在添加元素的时候，没有做判断，导致空元素也加到队列里，出现错误。

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

func levelOrder(root *TreeNode) []int {
    ans := []int{}
    if root == nil{
        return nil
    }
    queue := []*TreeNode{ root }
    for len(queue) > 0{
        ele :=  queue[0]
        queue = queue[1:]
        if ele != nil{
            ans = append(ans,ele.Val)
            if ele.Left != nil{
                queue = append(queue,ele.Left)
            }
            if ele.Right != nil{
                queue = append(queue,ele.Right)
            }
        }
    }
    return ans
}
```

# 思考

自己缺少了空节点的判断，实属是憨批了。