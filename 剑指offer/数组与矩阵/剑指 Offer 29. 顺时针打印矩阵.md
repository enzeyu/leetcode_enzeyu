# 剑指 Offer 29. 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```
示例 1：
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]

示例 2：
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

## 提示

- `0 <= matrix.length <= 100`
- `0 <= matrix[i].length <= 100`

# 自己的思路

把问题分解为子问题，即每次遍历增加一个圈，然后返回答案。

问题在于增加圆圈元素的时候自己卡住了，自己甚至想继续增加变量，很笨了这样。

# 我的代码(卡住了)

```go
func spiralOrder(matrix [][]int) []int {
    m, n := len(matrix), len(matrix[0])
    ans := []int{}
    for len(ans) != m*n{
        right,down := n,m
        up,left := m,n
        i_right, i_down := 0,
        for i := 0; i < n; i++{

        }
    }
}
```

# 更好的思路

## 方法一：不遍历到底

top < bottom && left < right 是循环的条件
无法构成“环”了，就退出循环，退出时可能是这 3 种情况之一：
top == bottom && left < right —— 剩一行
top < bottom && left == right —— 剩一列
top == bottom && left == right —— 剩一项（也是一行/列）



## 方法二：遍历到底

循环的条件改为： top <= bottom && left <= right
每遍历一条边，下一条边遍历的起点被“挤占”，要更新相应的边界。
需注意到，可能在循环途中，突然不再满足循环的条件，即top > bottom或left > right，其中一对边界彼此交错了
这意味着所有项都遍历完了，要break了，如果没有及时 break ，就会重复遍历

每遍历完一条边，更新相应的边界后，都加上一句if (top > bottom || left > right) break;，避免没有及时退出循环，导致重复遍历。
而且，遍历完成这个时间点，要么发生在遍历完“上边”，要么发生在遍历完“右边”
所以只需在这两步操作之后，加 if (top > bottom || left > right) break 即可

# 规范代码

```go
// 不遍历到底
func spiralOrder(matrix [][]int) []int {
    if len(matrix) == 0 {
        return []int{}
    }
    res := []int{}
    top, bottom, left, right := 0, len(matrix)-1, 0, len(matrix[0])-1
    
    for top < bottom && left < right {
        for i := left; i < right; i++ { res = append(res, matrix[top][i]) }
        for i := top; i < bottom; i++ { res = append(res, matrix[i][right]) }
        for i := right; i > left; i-- { res = append(res, matrix[bottom][i]) }
        for i := bottom; i > top; i-- { res = append(res, matrix[i][left]) }
        right--
        top++
        bottom--
        left++ 
    }
    // 如果顶端等于底部，则还有一行，否则则还有一列
    if top == bottom {
        for i := left; i <= right; i++ { res = append(res, matrix[top][i]) }
    } else if left == right {
        for i := top; i <= bottom; i++ { res = append(res, matrix[i][left]) }
    } 
    return res
}

// 遍历到底
func spiralOrder(matrix [][]int) []int {
    if len(matrix) == 0 {
        return []int{}
    }
    res := []int{}
    top, bottom, left, right := 0, len(matrix)-1, 0, len(matrix[0])-1
    
    for top <= bottom && left <= right {
        for i := left; i <= right; i++ { res = append(res, matrix[top][i]) }
        top++
        for i := top; i <= bottom; i++ { res = append(res, matrix[i][right]) }
        right--
        if top > bottom || left > right { break } 
        for i := right; i >= left; i-- { res = append(res, matrix[bottom][i]) }
        bottom--
        for i := bottom; i >= top; i-- { res = append(res, matrix[i][left]) }
        left++ 
    }

    return res
}
```

# 思考

https://leetcode-cn.com/problems/spiral-matrix/solution/shou-hui-tu-jie-liang-chong-bian-li-de-ce-lue-kan-/

这个题解是不错的，推荐解答1即不遍历完全。

