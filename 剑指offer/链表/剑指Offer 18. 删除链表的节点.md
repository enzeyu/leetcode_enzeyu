# 剑指Offer 18. 删除链表的节点

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

```
示例 1:
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.

示例 2:
输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
```

## 提示

- 题目保证链表中节点的值互不相同
- 若使用 C 或 C++ 语言，你不需要 `free` 或 `delete` 被删除的节点

# 自己的思路

常规删除链表，没啥好说的。

但是这里自己写的时候也遇到了问题，如果是原地删除，则要考虑首尾两端的情况。

# 我的代码

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteNode(head *ListNode, val int) *ListNode {
    temp := head
    if head.Val == val{
        head = head.Next
        return head
    }
    for temp.Next != nil{
        if temp.Next.Val == val{
            temp.Next = temp.Next.Next
        }
        temp = temp.Next
    }
    return head
}
```

# 更好的思路

最好的是使用一个哨兵节点，也就是这里的dummy。

## 方法二：双指针方法

设置两个指针分别指向头节点，pre （要删除节点的前一节点）和 cur (当前节点)；

遍历整个链表，查找节点值为 val 的节点，找到了就删除该节点，否则继续查找。

2.1. 找到，将当前节点的前驱节点（pre 节点或者说是之前最近一个值不等于 val 的节点）连接到当前节点（cur 节点）的下一个节点（pre->next = cur->next）。

2.2. 没找到，继续遍历（cur = cur->next），更新最近一个值不等于 val 的节点（pre = cur）。

# 规范代码

```go
// 单指针
func deleteNode(head *ListNode, val int) *ListNode {
    if head == nil {
        return nil
    }

    if head.Val == val {
        return head.Next
    }
	// 先寻找
    pre := head
    for pre.Next != nil && pre.Next.Val != val {
        pre = pre.Next
    }

    if pre.Next != nil {
        pre.Next = pre.Next.Next
    }

    return head
}

// 双指针
func deleteNode(head *ListNode, val int) *ListNode {
    if head == nil {
        return nil
    }

    if head.Val == val {
        return head.Next
    }

    pre, cur := head, head
    for cur != nil && cur.Val != val {
        pre, cur = cur, cur.Next
    }

    if cur != nil {
        pre.Next = cur.Next
    }

    return head
}

// 假头
func deleteNode(head *ListNode, val int) *ListNode {
    dummy := &ListNode{Val: 0, Next: head} // 假头
    first := dummy // 双指针1
    second := dummy.Next // 双指针2
    for {
        if second.Val == val {
            first.Next = second.Next
            break
        }
        first = first.Next
        second = second.Next
    }

    return dummy.Next
}
```

# 思考

这题自己思路没问题，但是就是没写出来，注意使用假头的写法。

