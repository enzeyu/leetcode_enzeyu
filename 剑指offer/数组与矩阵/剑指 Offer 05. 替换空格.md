# 剑指 Offer 05. 替换空格

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

```
示例 1：
输入：s = "We are happy."
输出："We%20are%20happy."
```

## 提示

- 0 <= s 的长度 <= 10000

# 自己的思路

构建返回字符串，遍历s，如果遍历到空格则补充%20，否则补充原始字符。

# 我的代码

```go
func replaceSpace(s string) string {
    ans := ""
    for _, i := range s{
        if string(i) == " "{
            ans += "%20"
        }else{
            ans += string(i)
        }
    }
    return ans
}
```

# 更好的思路



# 规范代码

```go

```

# 思考

简单题目。

