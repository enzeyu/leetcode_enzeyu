# 剑指 Offer 22. 链表中倒数第k个节点

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

```
示例：
给定一个链表: 1->2->3->4->5, 和 k = 2.
返回链表 4->5.
```

## 提示

# 自己的思路

输出倒数第几个节点，找到一个map，将顺序和映射存放进去，遍历完再返回。

# 我的代码

```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getKthFromEnd(head *ListNode, k int) *ListNode {
    dic := make(map[int]*ListNode,0)
    index := 1
    for head != nil{
        dic[index] = head
        index++
        head = head.Next
    }
    return dic[index-k]
}
```

# 更好的思路

## 方法二：双指针

快慢指针的思想。我们将第一个指针 fast 指向链表的第 k+1 个节点，第二个指针 slow 指向链表的第一个节点，此时指针 fast 与 slow 二者之间刚好间隔 k 个节点。此时两个指针同步向后走，当第一个指针 fast 走到链表的尾部空节点时，则此时 slow 指针刚好指向链表的倒数第k个节点。

我们首先将 fast 指向链表的头节点，然后向后走k 步，则此时fast 指针刚好指向链表的第k+1 个节点。

我们首先将slow 指向链表的头节点，同时slow 与fast 同步向后走，当fast 指针指向链表的尾部空节点时，则此时返回slow 所指向的节点即可。

# 规范代码

```go
func getKthFromEnd(head *ListNode, k int) *ListNode {
    fast, slow := head, head
    for fast != nil && k > 0 {
        fast = fast.Next
        k--
    }
    for fast != nil {
        fast = fast.Next
        slow = slow.Next
    }
    return slow
}
```

# 思考

双指针巧妙的，但是自己的也还行其实。