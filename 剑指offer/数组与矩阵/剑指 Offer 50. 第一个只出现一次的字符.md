# 剑指 Offer 50. 第一个只出现一次的字符

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```
示例 1:
输入：s = "abaccdeff"
输出：'b'

示例 2:
输入：s = "" 
输出：' '
```

## 提示

- 0 <= s 的长度 <= 50000

# 自己的思路

最直观的思路就是统计每个字母的出现频率，然后遍历map，寻找第一个出现一次的字符。

# 我的代码

```
func firstUniqChar(s string) byte {
    if len(s) == 0{
        return ' '
    }
    dic := make(map[rune]int)
    for _,i := range s{
        v, ok := dic[i]; if !ok{
            dic[i] = 1
        }else{
            if v >= 2{
                continue
            }
            dic[i] += 1
        }
    }

    for _,i := range s{
        if dic[i] == 1{
            return byte(i)
        }
    }
    return ' '
}
```



# 更好的思路



# 规范代码

```go

```

# 思考

应该不会出现在面试里。。。

