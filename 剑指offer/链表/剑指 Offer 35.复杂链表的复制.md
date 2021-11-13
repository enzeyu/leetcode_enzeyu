# 剑指 Offer 35.复杂链表的复制

请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

```
示例 1：
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

示例 2：
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]

示例 3：
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]

示例 4：
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。
```

## 提示

- `-10000 <= Node.val <= 10000`
- `Node.random` 为空（null）或指向链表中的节点。
- 节点数目不超过 1000 。

# 自己的思路

这道题的难点，在于构造新的复制的链表时，是无法保证指向的节点是存在的。

自己的思路在于先构造一个链表，把所有的节点都连接起来，然后再根据原始链表的信息找到新链表的对应节点，将链表节点指向对应节点。但是自己写了一半没写出来，属实难受。

# 我的代码

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 *     Random *Node
 * }
 */

func copyRandomList(head *Node) *Node {
    ans := &Node{head.Val,nil,nil}
    cop_ans := ans
    for head != nil{
        // 这一行报错，原因未知
        temp := &Node{head.Next.Val,nil,nil}
        cop_ans.Next = temp
        head = head.Next
        cop_ans = cop_ans.Next
    }
    return ans
}
```

# 更好的思路

## 方法一：回朔+哈希表

本题要求我们对一个特殊的链表进行深拷贝。如果是普通链表，我们可以直接按照遍历的顺序创建链表节点。而本题中因为随机指针的存在，当我们拷贝节点时，「当前节点的随机指针指向的节点」可能还没创建，因此我们需要变换思路。一个可行方案是，我们利用回溯的方式，让每个节点的拷贝操作相互独立。对于当前节点，我们首先要进行拷贝，然后我们进行「当前节点的后继节点」和「当前节点的随机指针指向的节点」拷贝，拷贝完成后将创建的新节点的指针返回，即可完成当前节点的两指针的赋值。

具体地，我们用哈希表记录每一个节点对应新节点的创建情况。遍历该链表的过程中，我们检查「当前节点的后继节点」和「当前节点的随机指针指向的节点」的创建情况。如果这两个节点中的任何一个节点的新节点没有被创建，我们都立刻递归地进行创建。当我们拷贝完成，回溯到当前层时，我们即可完成当前节点的指针赋值。注意一个节点可能被多个其他节点指向，因此我们可能递归地多次尝试拷贝某个节点，为了防止重复拷贝，我们需要首先检查当前节点是否被拷贝过，如果已经拷贝过，我们可以直接从哈希表中取出拷贝后的节点的指针并返回即可。

在实际代码中，我们需要特别判断给定节点为空节点的情况。

# 规范代码

```go
// 回朔+哈希表
var cachedNode map[*Node]*Node

func deepCopy(node *Node) *Node {
    if node == nil {
        return nil
    }
    if n, has := cachedNode[node]; has {
        return n
    }
    newNode := &Node{Val: node.Val}
    cachedNode[node] = newNode
    newNode.Next = deepCopy(node.Next)
    newNode.Random = deepCopy(node.Random)
    return newNode
}

func copyRandomList(head *Node) *Node {
    cachedNode = map[*Node]*Node{}
    return deepCopy(head)
}

// 方法二
func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    for node := head; node != nil; node = node.Next.Next {
        node.Next = &Node{Val: node.Val, Next: node.Next}
    }
    for node := head; node != nil; node = node.Next.Next {
        if node.Random != nil {
            node.Next.Random = node.Random.Next
        }
    }
    headNew := head.Next
    for node := head; node != nil; node = node.Next {
        nodeNew := node.Next
        node.Next = node.Next.Next
        if nodeNew.Next != nil {
            nodeNew.Next = nodeNew.Next.Next
        }
    }
    return headNew
}
```

# 思考

方法二是节省了空间，但是思路是一致的。这道题的递归写的挺好，需要再看。
