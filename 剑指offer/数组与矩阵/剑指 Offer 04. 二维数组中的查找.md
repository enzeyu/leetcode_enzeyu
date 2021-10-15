# 剑指 Offer 04. 二维数组中的查找

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数

```
现有矩阵 matrix 如下：
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。
给定 target = 20，返回 false。
```

## 提示

- 0<= n <= 1000
- 0<= m <= 1000

# 自己的思路

这道题之前写过，从左下出发，遇到小的则往上走，遇到更大的则往右走，越界的时候返回false即可。

# 我的代码

```go
func findNumberIn2DArray(matrix [][]int, target int) bool {
    if len(matrix) == 0 {
        return false
    }
    n, m := len(matrix), len(matrix[0])
    i, j := n-1, 0
    for i >=0 && j < m{
        if matrix[i][j] == target{
            return true
        } else if matrix[i][j] > target{
            i = i-1
        }else{
            j = j+1
        }
    }
    return false
}
```

# 更好的思路



# 规范代码

```go

```

# 思考

写过的题目了，拿下。

