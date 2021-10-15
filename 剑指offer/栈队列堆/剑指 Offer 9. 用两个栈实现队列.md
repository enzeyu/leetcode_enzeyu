# 剑指 Offer 9. 用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

```
示例 1：
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]

示例 2：
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

## 提示

- `1 <= values <= 10000`
- `最多会对 appendTail、deleteHead 进行 10000 次调用`

# 自己的思路

基础题了，实现队列不会有人不会吧。

# 我的代码

```
type CQueue struct {
    elements []int
    length int
}


func Constructor() CQueue {
    queue := CQueue{}
    return queue
}


func (this *CQueue) AppendTail(value int)  {
    this.elements = append(this.elements,value)
    this.length++
}


func (this *CQueue) DeleteHead() int {
    if this.length == 0{
        return -1
    }
    ans := this.elements[0]
    this.elements = this.elements[1:]
    this.length--
    return ans
}


/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */
```



# 更好的思路



# 规范代码

```go

```

# 思考

应该不会出现在面试里。。。

