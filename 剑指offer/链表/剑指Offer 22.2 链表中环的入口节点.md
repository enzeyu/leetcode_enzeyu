# 剑指 Offer 22. 链表中倒数第k个节点

给定一个链表，返回链表开始入环的第一个节点。 从链表的头节点开始沿着 next 指针进入环的第一个节点为环的入口节点。如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

```
示例 1：
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

## 提示

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

# 自己的思路

之前做过可以使用快慢指针判断是否有环，但是无法找到最新的位置。

为了解决问题，使用map结构保存每次的指针即可。

# 我的代码

```
func detectCycle(head *ListNode) *ListNode {
    dic := map[*ListNode]struct{}{}
    for head != nil{
        _,ok := dic[head]; if !ok{
            dic[head] = struct{}{}
        } else{
            return head
        }
        head = head.Next
    }
    return nil
}
```

# 更好的思路

快慢指针的思路，我们使用两个指针，fast 与 slow。它们起始都位于链表的头部。随后，slow 指针每次向后移动一个位置，而 fast 指针向后移动两个位置。如果链表中存在环，则 fast 指针最终将再次与 slow 指针在环中相遇。

**双指针第一次相遇：** 设两指针 `fast`，`slow` 指向链表头部 `head`，`fast` 每轮走 2 步，`slow` 每轮走 1 步；

第一种结果： fast 指针走过链表末端，说明链表无环，直接返回 null；

第二种结果：当 fast == slow时， 两指针在环中 第一次相遇 。下面分析此时 fast 与 slow走过的 步数关系 ：

- 设链表共有 a+b 个节点，其中 链表头部到链表入口 有 a 个节点（不计链表入口节点）， 链表环 有 b 个节点（这里需要注意，a 和 b 是未知数）；设两指针分别走了 f ，s 步，则有：
- fast 走的步数是 slow 步数的 2 倍，即 f = 2s；
- fast 比 slow 多走了 n 个环的长度，即 f = s + nb；（ 解析： 双指针都走过 a 步，然后在环内绕圈直到重合，重合时 fast 比 slow 多走 环的长度整数倍 ）；
  以上两式相减得：f=2nb，s=nb，即fast和slow 指针分别走了 2n，n 个 环的周长 （注意： n 是未知数，不同链表的情况不同）。

目前情况分析：

- 如果让指针从链表头部一直向前走并统计步数 k ，那么所有走到链表入口节点时的步数 是：k = a + nb（先走 a 步到入口节点，之后每绕 1 圈环（ b 步）都会再次到入口节点）。
- 而目前，slow 指针走过的步数为 nb 步。因此，我们只要想办法让 slow 再走 a 步停下来，就可以到环的入口。
- 但是我们不知道 a 的值，该怎么办？依然是使用双指针法。我们构建一个指针，此指针需要有以下性质：此指针和 slow 一起向前走 a 步后，两者在入口节点重合。那么从哪里走到入口节点需要 a 步？答案是链表头部 head 。

# 规范代码

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func detectCycle(head *ListNode) *ListNode {
    slow, fast := head, head
    for fast != nil {
        slow = slow.Next
        if fast.Next == nil {
            return nil
        }
        fast = fast.Next.Next
        if fast == slow {
            p := head
            for p != slow {
                p = p.Next
                slow = slow.Next
            }
            return p
        }
    }
    return nil
}
```

# 思考

这样 搞面试肯定挂了，双指针的证明比较复杂，但是很有用，需要复习！