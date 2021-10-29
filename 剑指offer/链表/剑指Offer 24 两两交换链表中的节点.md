# 剑指 Offer 24. 反转链表

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

```
示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

## 提示

- 0 <= 节点个数 <= 5000

# 自己的思路

想的当然尽量是原地反转，结果又GG了，没理清楚关系。

# 我的代码

```

```

# 更好的思路

在遍历链表时，将当前节点的 next 指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用。

# 规范代码

```go
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode
    curr := head
    for curr != nil {
        next := curr.Next
        curr.Next = prev
        prev = curr
        curr = next
    }
    return prev
}
```

# 思考

自己思考的时候思路很乱，需要理清楚迭代的想法。