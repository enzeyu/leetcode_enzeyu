# 剑指 Offer 30. 包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

```
示例：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```

## 提示

- 各函数的调用总次数不超过 20000 次

# 自己的思路

自己就是常规思路，只不过就是在min实现的时候，需要重新开辟一块空间，并调用排序函数。这样无法通过第17个测试用例。

# 我的代码(超出时间限制)

```
type MinStack struct {
    stack []int
    minStack []int
}

func Constructor() MinStack {
    return MinStack{
        stack: []int{},
        minStack: []int{math.MaxInt64},
    }
}

func (this *MinStack) Push(x int)  {
  	// 主栈直接添加
    this.stack = append(this.stack, x)
  	// 找到辅助栈的栈顶元素，比较后将更小的元素放入辅助栈
    top := this.minStack[len(this.minStack)-1]
    this.minStack = append(this.minStack, min(x, top))
}

func (this *MinStack) Pop()  {
  	// 同时弹出
    this.stack = this.stack[:len(this.stack)-1]
    this.minStack = this.minStack[:len(this.minStack)-1]
}

func (this *MinStack) Top() int {
  	// 返回最后一个元素
    return this.stack[len(this.stack)-1]
}

func (this *MinStack) Min() int {
    return this.minStack[len(this.minStack)-1]
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```



# 更好的思路

根据题意，我们需要在常量级的时间内找到最小值！

这说明，我们绝不能在需要最小值的时候，再做排序，查找等操作来获取！

所以，我们可以创建两个栈，一个栈是主栈 stack，另一个是辅助栈 minStack，用于存放对应主栈不同时期的最小值。

在辅助栈里，存放着主栈元素最小值。

# 规范代码

```go
type MinStack struct {
    stack []int
    minStack []int
}

func Constructor() MinStack {
    return MinStack{
        stack: []int{},
        minStack: []int{math.MaxInt64},
    }
}

func (this *MinStack) Push(x int)  {
  	// 主栈直接添加
    this.stack = append(this.stack, x)
  	// 找到辅助栈的栈顶元素，比较后将更小的元素放入辅助栈
    top := this.minStack[len(this.minStack)-1]
    this.minStack = append(this.minStack, min(x, top))
}

func (this *MinStack) Pop()  {
  	// 同时弹出
    this.stack = this.stack[:len(this.stack)-1]
    this.minStack = this.minStack[:len(this.minStack)-1]
}

func (this *MinStack) Top() int {
  	// 返回最后一个元素
    return this.stack[len(this.stack)-1]
}

func (this *MinStack) min() int {
    return this.minStack[len(this.minStack)-1]
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```

# 思考

应该不会出现在面试里。。。

