# 剑指 Offer 32.2 从上到下打印二叉树2

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

```
例如:
给定二叉树: [3,9,20,null,null,15,7],
返回其层次遍历结果：
[
  [3],
  [20,9],
  [15,7]
]
```

## 提示

- `节点总数 <= 1000`

# 自己的思路

返回值是二维数组，所以每次查找的时候都必须要把当前层的数据及时加入到ans里。

这里很容易发现，奇数层就是从左到右的顺序，偶数层是从右到左，故每次需要记录层数。

# 我的代码(第20个用例出错)

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
    if root == nil{
        return nil
    }
    var ans [][]int
    oddQueue := []*TreeNode{ root }
    evenQueue := []*TreeNode{}
    layer := 1
    for len(oddQueue) > 0 || len(evenQueue) > 0{
        // 奇数层
        elements := []int{}
        if layer % 2 == 1{
            n := len(oddQueue)
            for i := 0; i < n; i++{
                node := oddQueue[0]
                oddQueue = oddQueue[1:]
                if node != nil{
                    elements = append(elements,node.Val)
                }
                if node.Left != nil{
                    evenQueue = append(evenQueue,node.Left)
                }
                if node.Right != nil{
                    evenQueue = append(evenQueue,node.Right)
                }
            }
            layer++
        }else{ // 偶数层
            n := len(evenQueue)
            for i := 0; i < n; i++{
                curLen := len(evenQueue)
                node := evenQueue[curLen-1]
                evenQueue = evenQueue[:curLen-1]
                if node != nil{
                    elements = append(elements,node.Val)
                }
                if node.Left != nil{
                    oddQueue = append(oddQueue,node.Left)
                }
                if node.Right != nil{
                    oddQueue = append(oddQueue,node.Right)
                }
            }
            layer++
        }
        ans = append(ans,elements)

    }
    return ans
}
```

# 更好的思路

## 方法一：双端队列+层序遍历

- 利用双端队列的两端皆可添加元素的特性，设打印列表（双端队列） `tmp` ，并规定

  - 奇数层：添加到tmp尾部
  - 偶数层：添加到tmp头部

- 算法流程

  - 特例处理： 当树的根节点为空，则直接返回空列表 [] ；

  - 初始化： 打印结果空列表 res ，包含根节点的双端队列 deque ；

  - BFS 循环： 当 deque 为空时跳出；

    - 新建tmp存储当前层打印结果
    - 当前层打印循环：循环次数为当前层节点数
      - 出队：队首元素出列记为node
      - 打印：奇数层则将node.Val添加到tmp尾部，否则tmp头部
      - 添加子节点：如果node左右节点不为空，则加入dequeue

  - 将tmp添加到res

  - 返回res

    

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
    if root == nil{
        return nil
    }
    var ans [][]int
    deQueue := []*TreeNode{ root }
    layer := 1
    for len(deQueue) > 0 {
        elements := []int{}
        n := len(deQueue)
        for i := 0; i < n; i++{
            node := deQueue[0]
            deQueue = deQueue[1:]
            if node != nil && layer % 2 == 1{
                elements = append(elements,node.Val)
            } else{
                elements = append([]int{node.Val},elements...)
            }
            if node.Left != nil{
                deQueue = append(deQueue,node.Left)
            }
            if node.Right != nil{
                deQueue = append(deQueue,node.Right)
            }
        }
        layer++
        ans = append(ans,elements)
    }
    return ans
}
```

# 思考

使用双端队列的方法很巧妙，值得复习。

