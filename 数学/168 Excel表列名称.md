# Excel表列名称

给你一个整数columnNumber，返回它在Excel表里对应的列名称。

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

```
示例 1：
输入：columnNumber = 1
输出："A"

示例 2：
输入：columnNumber = 28
输出："AB"

示例 3：
输入：columnNumber = 701
输出："ZY"

示例 4：
输入：columnNumber = 2147483647
输出："FXSHRXW"
```

## 提示

- `1 <= columnNumber <= 231 - 1`

# 自己的思路

迭代循环columnNumber，逐个进行添加，但是此时遇到整除columnNumber的就无法通过。

# 我的代码

```go
func convertToTitle(columnNumber int) string {
    ans := ""
    helper := "ABCDEFGHIJKLMNOPQISTUVWXYZ"
    for columnNumber > 0{
        ans = string(helper[columnNumber%26-1]) + ans 
        columnNumber = columnNumber / 26
    }
    return ans
}
```

# 更好的思路

这里给的思路和自己想的一样，但是细节把握的更好，使用+'A'的形式即可。

# 规范代码

```go
func convertToTitle(columnNumber int) string {
    ans := ""
    for columnNumber > 0{
        columnNumber -= 1
        ans = string(columnNumber%26 + 'A') + ans 
        columnNumber = columnNumber / 26
    }
    return ans
}
```

# 思考

遇到构建字符，需要想到整数+字母的形式。

