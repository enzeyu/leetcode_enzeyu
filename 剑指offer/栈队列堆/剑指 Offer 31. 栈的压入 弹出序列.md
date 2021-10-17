# 剑指 Offer 31. 栈的压入 弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

```
示例 1：
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1

示例 2：
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

## 提示

- 0 <= pushed.length == popped.length <= 1000
- 0 <= pushed[i], popped[i] < 1000
- pushed 是 popped 的排列。

# 自己的思路

记得学数据结构的时候写过这题，但是自己现在没写出来。

# 我的代码

```

```



# 更好的思路

使用辅助栈，模拟一遍弹出的操作

每插入一个元素到辅助栈中，判断是否可进行弹出操作

直到弹出全部元素，如果能够弹出，说明成功，否则失败。

# 规范代码

```go
func validateStackSequences(pushed []int, popped []int) bool {
    stack := []int{}
    i := 0
    for _, v := range pushed {
        stack = append(stack, v)
        for len(stack) > 0 && stack[len(stack) - 1] == popped[i] {
            stack = stack[:len(stack) - 1]
            i++
        }
    }
    return !(len(stack) > 0)
}

```

# 思考

这道题...确实巧妙，需要继续复习。

