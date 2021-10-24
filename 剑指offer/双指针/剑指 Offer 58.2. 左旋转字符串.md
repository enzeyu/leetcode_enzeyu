# 剑指 Offer 58.2. 左旋转字符串

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

```
示例 1：
输入: s = "abcdefg", k = 2
输出: "cdefgab"

示例 2：
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```

## 提示

- `1 <= k < s.length <= 10000`

# 自己的思路

使用切片，将字符串进行重组即可。

# 我的代码

```
func reverseLeftWords(s string, n int) string {
    ans := s[n:] + s[:n]
    return ans 
}
```

# 更好的思路

谈不上更好，下一个思路就是用双指针进行反转，三部分的反转即可。

# 规范代码

```go
// 三步反转法
func reverseLeftWords(s string, n int) string {
	str := []rune(s)
	reverse(str[:n])
	reverse(str[n:])
	reverse(str)
	return string(str)
}

func reverse(str []rune) {
	i, j := 0, len(str)-1
	for i < j {
		str[i], str[j] = str[j], str[i]
		i++
		j--
	}
}
```

# 思考

虽然直接拼接的思路时间和效率都高，但是也要注意双指针的方法。