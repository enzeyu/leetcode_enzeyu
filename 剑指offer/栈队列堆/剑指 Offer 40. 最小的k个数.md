# 剑指 Offer 40. 最小的k个数

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

```
示例 1：
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]

示例 2：
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

## 提示

- `0 <= k <= arr.length <= 10000`
- `0 <= arr[i] <= 10000`

# 自己的思路

这道题应该是要用堆，但是自己只会排序后直接返回对应的索引。

# 我的代码

```
func getLeastNumbers(arr []int, k int) []int {
    sort.Slice(arr,func (a,b int)bool{
        return arr[a] < arr[b]
    })
    return arr[:k]
}
```

# 更好的思路

## 方法一：使用堆

时间复杂度 O(n log k)

使用堆数据结构来辅助得到最小的 k 个数。堆的性质是每次可以找出最大或最小的元素。可以使用一个大小为 k 的最大堆（大顶堆），将数组中的元素依次入堆，当堆的大小超过 k 时，便将多出的元素从堆顶弹出。

**每次从堆顶弹出的数都是堆中最大的，最小的 k 个元素一定会留在堆里**。这样，把数组中的元素全部入堆之后，堆中剩下的 k 个元素就是最大的 k 个数了。

注意，我们并没有给出堆的内部结构，因为这部分内容并不重要。我们只需要知道堆每次会弹出最大的元素即可。

## 方法二：快排

时间复杂度 O(n)

找第 k 大的数，或者找前 k 大的数，有一个经典的 quick select（快速选择）算法。这个名字和 quick sort（快速排序）看起来很像，算法的思想也和快速排序类似，都是分治法的思想。

这个 partition 操作是原地进行的，需要 O(n) 的时间，接下来，快速排序会递归地排序左右两侧的数组。而快速选择（quick select）算法的不同之处在于，接下来只需要递归地选择一侧的数组。快速选择算法想当于一个“不完全”的快速排序，因为我们只需要知道最小的 k 个数是哪些，并不需要知道它们的顺序。

我们的目的是寻找最小的 k 个数。假设经过一次 partition 操作，枢纽元素位于下标 m，也就是说，左侧的数组有 m 个元素，是原数组中最小的 m 个数。那么：

若 k=m，我们就找到了最小的 k 个数，就是左侧的数组；
若 k<m ，则最小的 k 个数一定都在左侧数组中，我们只需要对左侧数组递归地 parition 即可；
若 k>m，则左侧数组中的 m 个数都属于最小的 k 个数，我们还需要在右侧数组中寻找最小的k−m 个数，对右侧数组递归地 partition 即可。



# 规范代码

```go
func getLeastNumbers(arr []int, k int) []int {
    if len(arr)==0 || k==0 {
        return nil
    }
    d:=&heapInt{}
    // heap.Init(d)
    for _,v:=range arr {
        // 初始化往结果里增加元素
        if d.Len()<k {
            heap.Push(d,v)
        }else {
            // 堆顶元素大于当前元素
            if (*d)[0]>v {
                heap.Pop(d)
                heap.Push(d,v)
            }
        }
    }
    return *d
}

type heapInt []int

//Less  小于就是小跟堆，大于号就是大根堆
func (h *heapInt)Less(i,j int)bool {return (*h)[i]>(*h)[j]}
func (h *heapInt)Swap(i,j int) {(*h)[i],(*h)[j]=(*h)[j],(*h)[i]}
func (h *heapInt)Len() int {return len(*h)}
// Push and Pop use pointer receivers because they modify the slice's length,
// not just its contents.
func (h *heapInt)Push(x interface{}) {
	*h=append(*h,x.(int))
}
// 官方注释可知，我们只需要将切片变为其[0:n-1]即可，
func (h *heapInt)Pop() interface{} {
	t:=(*h)[len(*h)-1]
	*h=(*h)[:len(*h)-1]
	return t
}





// 普通快排
func getLeastNumbers(arr []int, k int) []int {
    if len(arr)==0 || k==0 {
        return nil
    }

    quicksort(arr, 0, len(arr)-1)
    return arr[:k]

}

func partition(nums []int,i,j int) int {
    l,m,r:=i,i,j
    for l<r {
        for l<r && nums[r]>=nums[m] {
                r--
        }
        for l<r && nums[l]<=nums[m] {
                l++
        }
        if l<r {
            nums[l],nums[r]=nums[r],nums[l]
        }
    }
    nums[m],nums[l]=nums[l],nums[m]
    
    return l
}

func quicksearch(nums []int,i,j,k int) []int{    
    t:=partition(nums,i,j)
    if t==k-1 {
        return nums[:k]
    }
    if t<k-1 {
        return quicksearch(nums,t+1,j,k)
    }
    return quicksearch(nums,i,t-1,k)
}

func quicksort(nums []int,i,j int) {
    if i>=j {
        return
    }
    m:=partition(nums, i,j)
    quicksort(nums,i,m-1)
    quicksort(nums,m+1,j)
}


// 改良的快排
import "math/rand"

var kk int
func getLeastNumbers(arr []int, k int) []int {
   kk=k
   quickSort(arr,0,len(arr)-1)
   return arr[:k]
}
func quickSort(nums[]int,left,right int)bool{
    if left>right{
        return false
    }
    key:=rand.Int()%(right-left+1)+left
    nums[left],nums[key]=nums[key],nums[left]
    i,j,pivot:=left,right,nums[left]
    for i<j{
        for i<j&&nums[j]>=pivot{ //如果是求前k大,这里nums[j]>=pivot改成 nums[j]<=pivot
            j--
        }
        for i<j&&nums[i]<=pivot{//如果是求前k大,这里nums[i]<=pivot改成 nums[i]>=pivot即可
            i++
        }
        nums[i],nums[j]=nums[j],nums[i]
    }
    nums[left],nums[i]=nums[i],nums[left]
     if kk<=left{//left之前的元素都已经有序了,那么当kk<=left时直接return即可
         return true
     }
     if quickSort(nums,left,i-1){
         return true
     }
     if quickSort(nums,i+1,right){
         return true
     }     
     return false
}

```

# 思考

学习快排或者大根堆的解，特别注意go里堆的实现，需要实现五个函数，以完成实现"接口"的意思。https://blog.csdn.net/weixin_40631132/article/details/105208272

快排需要再想。

