# 28 实现strStr()

实现 strStr() 函数。

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  -1 。

说明：

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与 C 语言的 strstr() 以及 Java 的 indexOf() 定义相符。

```
示例 1：
输入：haystack = "hello", needle = "ll"
输出：2

示例 2：
输入：haystack = "aaaaa", needle = "bba"
输出：-1

示例 3：
输入：haystack = "", needle = ""
输出：0
```

## 提示

- `0 <= haystack.length, needle.length <= 5 * 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

# 自己的思路

使用双指针i,j分别指向两个字符串haystack和needle，外层循环对haystack进行循环。

当haystack[i]不等于needle[j]的时候，haystack向后+1

当haystack[i]等于needle[j]，两个指针同时往后，如果最后j==len(needle)-1，那么返回i。否则将j置为0，继续下次循环，最终如果没用找到则返回-1。



自己一开始想的没办法通过misip测试用例，换方法为，将字符串比抽象为函数，然后遍历每一个strStr的每一个字符，使用切片取局部字符串放入函数对比。

# 我的代码

```go
func strStr(haystack string, needle string) int {
    if needle == ""{
        return 0
    }
    j := 0
    for i := 0; i < len(haystack); i++{
        if haystack[i] != needle[j]{
            j = 0
            continue
        }else{
            j++
            if j == len(needle){
                return i-j+1
            }
        }

    }
    return -1
}

// 思路2，类似于枚举了
func strStr(haystack string, needle string) int {
    if needle == ""{
        return 0
    }
    if haystack == ""{
        return -1
    }
    for i := 0; i <= len(haystack)-len(needle); i++{
        if isEqual(haystack[i:i+len(needle)],needle){
            return i
        }
    }
    return -1
}

func isEqual(a string,b string) bool{
    for i := 0; i < len(a); i++{
        if a[i] != b[i]{
            return false
        }
    }
    return true
}
```

# 更好的思路

本题是KMP经典，KMP思想就是：当字符串不匹配时，记录一部分之前匹配的文本内容，利用这些信息避免从头匹配，使用next数组作为前缀表，记录已经匹配的文本内容。

## 前缀表

前缀表用于回退，记录了 模式串和主串不匹配的时候，模式串应该从哪里继续匹配。

举例：要在文本串：aabaabaafa 中查找是否出现过一个模式串：aabaaf。前缀表的任务是当前位置匹配失败，找到之前已经匹配上的位置，再重新匹配，此也意味着在某个字符失配时，前缀表会告诉你下一步匹配中，模式串应该跳到哪个位置。

故前缀表的概念是，**记录下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。**

# 规范代码

```go
func strStr(haystack, needle string) int {
	//KMP算法
	n, m := len(haystack), len(needle)
	//排除 needle=""
	if m == 0 {
		return 0
	}
	//next数组表示模式串当前位(从0数)的最长公共前后缀
	next := make([]int, m)
	for i, j := 1, 0; i < m; i++ { //i指向后缀,j指向前缀
		for j > 0 && needle[i] != needle[j] { //注意for
			//j回到第第next[j-1]位(前缀(子串)的最长公共前后缀)开始比较
			j = next[j-1]
		}
		if needle[i] == needle[j] {
			j++
		}
		next[i] = j
	}
	//遍历haystack
	for i, j := 0, 0; i < n; i++ {
		for j > 0 && haystack[i] != needle[j] { //注意for
			//如果发生不匹配，则回到前一位的最长公共前后缀位继续匹配，
			//如匹配不成功则重复此过程，直到匹配成功或者回到模式串第0个位置
			//如果有过多重复字符,则会造成无意义的比较,见kmp的改进
			j = next[j-1]
		}
		if haystack[i] == needle[j] {
			j++
		}
		if j == m {
			return i - m + 1
		}
	}
	return -1
}
```

# 思考

自己想的方法没有通过测试用例"mississippi"和"issip"，这是因为在判断前面的时候用到了之后的字符。

KMP的方法值得复习。