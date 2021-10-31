# 剑指 Offer 25 合并两个排序的链表

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

```
示例1：
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

## 提示

- 0 <= 链表长度 <= 1000

# 自己的思路

先使用迭代的方法进行解决，找到一个伪头节点dummy，然后使用双指针分别遍历两个节点，谁小就添加到dummy的后面，最后返回dummy即可。

# 我的代码

```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{-1,nil}
    curr := dummy
    for l1 != nil && l2 != nil{
        if l1.Val <= l2.Val{
            curr.Next = l1
            l1 = l1.Next
        } else{
            curr.Next = l2
            l2 = l2.Next
        }
        curr = curr.Next
    }
    if l1 != nil{
        curr.Next = l1
    }
    if l2 != nil{
        curr.Next = l2
    }
    return dummy.Next
}
```

# 更好的思路

运行时间来看，还是迭代好用，但是递归的思路需要掌握。

递归的退出条件就是，当某一方为空的时候返回另一个链表节点。而如果l1的值小于l2，相当于取l1的值作为节点，再去看l1.Next和l2的值。这样就很好理解了。

# 规范代码

```go
// 递归
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 == nil{ return l2 }
    if l2 == nil{ return l1 }
    if l1.Val <= l2.Val{
        l1.Next = mergeTwoLists(l1.Next,l2)
        return l1
    }else{
        l2.Next = mergeTwoLists(l2.Next,l1)
        return l2
    }
}
```

# 思考

递归要学会！