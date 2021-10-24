# 剑指 Offer 58.1 翻转单词顺序列

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

```
示例 1：
输入: "the sky is blue"
输出: "blue is sky the"

示例 2：
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

示例 3：
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

## 提示

- 无空格字符构成一个单词。

- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

  

# 自己的思路

使用字符串进行分割，然后逆序分割后的数组返回字符串即可。

# 我的代码

```
func reverseWords(s string) string {
	words := regexp.MustCompile(`\s+`).Split(strings.Trim(s, " "), -1)
	for i, j := 0, len(words)-1; i < j; i, j = i+1, j-1 {
		words[i], words[j] = words[j], words[i]
	}
	return strings.Join(words, " ")
}
```



# 更好的思路

## 分割合并

这里的分割合并没有使用正则表达式，见规范代码1。

# 规范代码

```go
func reverseWords(s string) string {
	strList := strings.Split(s," ")
	var res []string
	for i :=len(strList)-1;i>=0;i--{
		str := strings.TrimSpace(strList[i])
		if  len(str)>0 {
			res = append(res,strList[i])
		}
	}
	return strings.Join(res," ")
}
```

# 思考

这里可以用双指针，但是太麻烦了。