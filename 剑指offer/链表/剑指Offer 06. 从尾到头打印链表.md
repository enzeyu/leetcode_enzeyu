# 剑指 Offer 06. 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

```
示例 1：
输入：head = [1,3,2]
输出：[2,3,1]
```

## 提示

- 0 <= 链表长度 <= 10000

# 自己的思路

顺序存放到数组，再对数组进行逆序。

# 我的代码

```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reversePrint(head *ListNode) []int {
    ans := []int{}
    for head != nil{
        ans = append(ans,head.Val)
        head = head.Next
    }
    for i,j := 0,len(ans)-1; i<j; i,j = i+1,j-1{
        ans[j],ans[i] = ans[i],ans[j]
    }
    return ans
}
```



# 更好的思路

## 方法一：递归

逆向递归既可以解决问题，很巧妙，结合代码看一下。

# 规范代码

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reversePrint(head *ListNode) ([]int) {
	if head == nil {return nil}
	return append(reversePrint(head.Next), head.Val)
}

```

# 思考

使用递归逆序的方法很巧妙。