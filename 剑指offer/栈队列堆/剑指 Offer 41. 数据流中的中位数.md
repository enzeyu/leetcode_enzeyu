# 剑指 Offer 41. 数据流中的中位数

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。

```
示例 1：
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]

示例 2：
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```

## 提示

- 最多会对 `addNum、findMedian` 进行 `50000` 次调用。

# 自己的思路

题目一看就是要用堆了，难度在于如何使用一个长度变化的堆，并及时返回堆的中位数。

把堆的实现弄上去。

# 我的代码

```go

```

# 更好的思路

建立一个 **小顶堆** A 和 **大顶堆** B，各保存列表的一半元素，且规定：

- A保存较大的一半，长度为N/2（N为偶数）或者（N+1）/2（N为奇数）

- A保存较大的一半，长度为N/2（N为偶数）或者（N-1）/2（N为奇数）

- a_1<......<a_m（小顶堆堆顶最小，存储较大元素）

- b_1>......b_n（大堆堆堆顶最大，存储较小元素）

  ![Picture1.png](https://pic.leetcode-cn.com/25837f1b195e56de20587a4ed97d9571463aa611789e768914638902add351f4-Picture1.png)

  **算法流程：**

  设元素总数为 N = m + n ，其中 m和 n 分别为 A 和 B 中的元素个数。

  **addNum(num)函数：**

  当 m = n（即 N 为 偶数）：需向 A 添加一个元素。实现方法：将新元素 num 插入至 B ，再将 B堆顶元素插入至 A ；
  当 m !=n（即 N为 奇数）：需向 B 添加一个元素。实现方法：将新元素 num 插入至 A ，再将 A堆顶元素插入至 B ；

  注：假设插入数字 num 遇到情况 1. 。由于 num 可能属于 “较小的一半” （即属于 B ），因此不能将 num 直接插入至 A 。而应先将 num 插入至 B ，再将 B 堆顶元素插入至 A 。这样就可以始终保持 A 保存较大一半、 B 保存较小一半。

  

  

# 规范代码

```go
type maxHeap []int  // 大顶堆
type minHeap []int  // 小顶堆

// 每个堆都要heap.Interface的五个方法：Len, Less, Swap, Push, Pop

// Len 返回堆的大小
func (m maxHeap) Len() int {
	return len(m)
}
func (m minHeap) Len() int {
	return len(m)
}

// Less 决定是大优先还是小优先
func (m maxHeap) Less(i, j int) bool {  // 大根堆
	return m[i] > m[j]
}
func (m minHeap) Less(i, j int) bool {  // 小根堆
	return m[i] < m[j]
}

 // Swap 交换下标i, j元素的顺序
func (m maxHeap) Swap(i, j int) {  
	m[i], m[j] = m[j], m[i]
}
func (m minHeap) Swap(i, j int) {   
	m[i], m[j] = m[j], m[i]
}

// Push 在堆的末尾添加一个元素，注意和heap.Push(heap.Interface, interface{})区分
func (m *maxHeap) Push(v interface{}) {
	*m = append(*m, v.(int))
}
func (m *minHeap) Push(v interface{}) {
	*m = append(*m, v.(int))
}

// Pop 删除堆尾的元素，注意和heap.Pop()区分
func (m *maxHeap) Pop() interface{} {
	old := *m
	n := len(old)
	v := old[n - 1]
	*m = old[:n - 1]
	return v
}
func (m *minHeap) Pop() interface{} {
	old := *m
	n := len(old)
	v := old[n - 1]
	*m = old[:n - 1]
	return v
}

// MedianFinder 维护两个堆，前一半是大顶堆，后一半是小顶堆，中位数由两个堆顶决定
type MedianFinder struct {	
	maxH *maxHeap
	minH *minHeap
}

// Constructor 建两个空堆
func Constructor() MedianFinder {
	return MedianFinder{
		new(maxHeap),
		new(minHeap),
	}
}

// AddNum 插入元素num
// 分两种情况插入：
// 1. 两个堆的大小相等，则小顶堆增加一个元素（增加的不一定是num）
// 2. 小顶堆比大顶堆多一个元素，大顶堆增加一个元素
// 这两种情况又分别对应两种情况：
// 1. num小于大顶堆的堆顶，则num插入大顶堆
// 2. num大于小顶堆的堆顶，则num插入小顶堆
// 插入完成后记得调整堆的大小使得两个堆的容量相等，或小顶堆大1
func (m *MedianFinder) AddNum(num int)  {
	if m.maxH.Len() == m.minH.Len() {
        // 如果大于小堆顶，直接加入小顶堆，但是这里的m.minH.Len() == 0保证小顶堆大1
		if m.minH.Len() == 0 || num >= (*m.minH)[0] {
			heap.Push(m.minH, num)
		} else {
			heap.Push(m.maxH, num)
			top := heap.Pop(m.maxH).(int)
			heap.Push(m.minH, top)
		}
	} else {
		if num > (*m.minH)[0] {
			heap.Push(m.minH, num)
			bottle := heap.Pop(m.minH).(int)
			heap.Push(m.maxH, bottle)
		} else {
			heap.Push(m.maxH, num)
		}
	}
}

// FindMediam 输出中位数
func (m *MedianFinder) FindMedian() float64 {
	if m.minH.Len() == m.maxH.Len() {
		return float64((*m.maxH)[0]) / 2.0 + float64((*m.minH)[0]) / 2.0
	} else {
		return float64((*m.minH)[0])
	}
}
```

# 思考

使用两个堆即可，这个自己没想到的。解析可见https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/solution/mian-shi-ti-41-shu-ju-liu-zhong-de-zhong-wei-shu-y/

