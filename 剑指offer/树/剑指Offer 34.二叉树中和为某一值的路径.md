# 剑指 Offer 34.二叉树中和为某一值的路径

给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

```
示例 1：
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]

示例 2:
输入：root = [1,2,3], targetSum = 5
输出：[]

示例 3：
输入：root = [1,2], targetSum = 0
输出：[]
```

## 提示

- 树中节点总数在范围 `[0, 5000]` 内
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

# 自己的思路

自己一开始的想法是计算每个根节点开始的路径，判断是否等于目标值，但是这样显然蠢了点。

# 我的代码

```

```



# 更好的思路

## 方法一：先序遍历+路径记录

- pathSum(root,sum)函数
  - 初始化：结果列表res，路径列表path
  - 返回值：返回res
- recur(root, tar) 函数
  - 递推参数：当前节点root，当前目标值tar
  - 终止条件：当root为空，直接返回
  - 递推工作
    - 路径更新：将当前节点值加入路径
    - 目标值更新：tar = tar-root.Val
    - 路径记录：当root为叶节点且路径和等于目标值，则将路径path加入res
    - 先序遍历：递归左右子节点
    - 路径恢复：即将当前节点从路径删除，path.pop()

# 规范代码

```go
func pathSum(root *TreeNode, sum int) [][]int {
    if root == nil {
        return nil
    }
    var ret [][]int
    dfs(root,sum,[]int{},&ret)
    return ret
}

func dfs(root *TreeNode,sum int,arr []int,ret *[][]int){
    if root == nil{
        return
    }
    arr = append(arr,root.Val)

    if root.Val == sum && root.Left == nil && root.Right == nil {
        //slice是一个指向底层的数组的指针结构体
        //因为是先序遍历，如果 root.Right != nil ,arr 切片底层的数组会被修改
        //所以这里需要 copy arr 到 tmp，再添加进 ret，防止 arr 底层数据修改带来的错误
        // 也可以将这三行写成*ret = append(*ret, append([]int{}, arr...))
        tmp := make([]int,len(arr))
        copy(tmp,arr)
        *ret = append(*ret,tmp)
    }

    dfs(root.Left,sum - root.Val,arr,ret)
    dfs(root.Right,sum - root.Val,arr,ret)

    arr = arr[:len(arr)-1]
}

// 代码2
func pathSum(root *TreeNode, target int) (ans [][]int) {
    path := []int{} //初始化存储路径
    var dfs func(*TreeNode, int)
    dfs = func(node *TreeNode, left int) {  //先序递归函数
        if node == nil {
            return
        }
        left -= node.Val                   //减去当前节点值
        path = append(path, node.Val)       //将节点加入路径
        if node.Left == nil && node.Right == nil && left == 0 { //叶节点，且和为tar
            ans = append(ans, append([]int{}, path...))     //路径添加到结果
            path = path[:len(path)-1]                       //return前要删去路径中当前节点
            return
        }
        dfs(node.Left, left)                                //递归左子树
        dfs(node.Right, left)                               //递归右子树
        path = path[:len(path)-1]                       //递归完成后返回上层前删去路径中当前节点
    }
    dfs(root, target)
    return
}
```

# 思考

发现自己审题也不行，题目要求的是找根节点到叶节点的路径。