# 剑指 Offer 32.2 从上到下打印二叉树2

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

```
例如:
给定二叉树: [3,9,20,null,null,15,7],
返回其层次遍历结果：
[
  [3],
  [9,20],
  [15,7]
]
```

## 提示

- `节点总数 <= 1000`

# 自己的思路

自己刚拿题的时候，感觉这道题和昨天的几乎一样，只是返回的值是一个二维数组。

然后发现并不是这样，因为这里用的是二维数组，所以每次查找的时候都必须要满足把当前层及时加入到ans里。



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
func levelOrder(root *TreeNode) [][]int {
    var ans [][]int
    queue := []*TreeNode{ root }
    for len(queue) != 0 {
        n := len(queue)
        var currentLayer []int
        for i := 0; i < n; i++{
            node := queue[0]
            queue = queue[1:]
            if node != nil{
                currentLayer = append(currentLayer,node.Val)
                if node.Left != nil{
                    queue = append(queue,node.Left)
                }
                if node.Right != nil{
                    queue = append(queue,node.Right)
                }
            }
        }
        if len(currentLayer) > 0{
            ans = append(ans,currentLayer)
        }
    }
    return ans
}
```

# 更好的思路

每次需要每一层的长度，并进行添加。

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
func levelOrder(root *TreeNode) [][]int {
    var res [][]int
    var stack []*TreeNode
    stack = append(stack, root)
    for len(stack) != 0 {
        n := len(stack)
        var v []int
        for i := 0;i < n;i ++ {  
            node := stack[0]
            stack = stack[1:]
            if node != nil {
                v = append(v, node.Val)
                if node.Left != nil {
                    stack = append(stack, node.Left)
                }
                if node.Right != nil {
                    stack = append(stack, node.Right)
                }
            }
        }
        if len(v) > 0 {
            res = append(res, v)
        }
    }
    return res
}
```

# 思考

自己没有想到每次要记录当前层的数目，感觉乱了，后来自己修改的代码和样例的没啥区别，但是就是运行不出来？

