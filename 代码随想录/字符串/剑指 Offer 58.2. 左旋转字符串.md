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

第二次遇到了，这次不用拼接切片来做了。

使用三次翻转，即第一次以n分割两个字符串进行分割，然后翻转整个字符串。

# 我的代码

```
func reverseLeftWords(s string, n int) string {
    bytes := []byte(s)
    reverse := func(s []byte, left,right int) {
        for left < right{
            s[left],s[right] = s[right],s[left]
            left++
            right--
        }
    }
    reverse(bytes,0,n-1)
    reverse(bytes,n,len(s)-1)
    reverse(bytes,0,len(s)-1)
    return string(bytes)
}
```

# 更好的思路



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

注意翻转的次序。