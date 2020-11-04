### date 2020-11-1
#### [1512. 好数对的数目](https://leetcode-cn.com/problems/number-of-good-pairs/)

```
给你一个整数数组 nums 。

如果一组数字 (i,j) 满足 nums[i] == nums[j] 且 i < j ，就可以认为这是一组 好数对 。

返回好数对的数目。

```
```
示例 1：

输入：nums = [1,2,3,1,1,3]
输出：4
解释：有 4 组好数对，分别是 (0,3), (0,4), (3,4), (2,5) ，下标从 0 开始
示例 2：

输入：nums = [1,1,1,1]
输出：6
解释：数组中的每组数字都是好数对
示例 3：

输入：nums = [1,2,3]
输出：0
```

> 分析：要满足 i < j 条件，所以可以两层循环，j=i+1

```Golang
func numIdenticalPairs(nums []int) int {
    total := 0
    numsLen := len(nums)

    for i := 0; i < numsLen; i++ {
        for j := i + 1; j < numsLen;j++ {
            if nums[i] == nums[j] {
                total++
            }
        }
    }
    return total
}
```


### date 2020-11-2
#### [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

```
给定两个数组，编写一个函数来计算它们的交集。

输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]

```

> 两个数组都做hash映射，再判断
```Golang
func intersection(nums1 []int, nums2 []int) (intersection []int) {
    set1 := map[int]struct{}{}
    for _, v := range nums1 {
        set1[v] = struct{}{}
    }
    set2 := map[int]struct{}{}
    for _, v := range nums2 {
        set2[v] = struct{}{}
    }
    if len(set1) > len(set2) {
        set1, set2 = set2, set1
    }
    for v := range set1 {
        if _, has := set2[v]; has {
            intersection = append(intersection, v)
        }
    }
    return
}
```

### date 2020-11-3
#### [941. 有效的山脉数组](https://leetcode-cn.com/problems/valid-mountain-array/)
```
给定一个整数数组 A，如果它是有效的山脉数组就返回 true，否则返回 false。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：

A.length >= 3
在 0 < i < A.length - 1 条件下，存在 i 使得：
        A[0] < A[1] < ... A[i-1] < A[i]
        A[i] > A[i+1] > ... > A[A.length - 1]。
```
![](https://assets.leetcode.com/uploads/2019/10/20/hint_valid_mountain_array.png)

```
输入：[2,1]
输出：false

输入：[3,5,5]
输出：false

输入：[0,3,2,1]
输出：true
```
> 分析：确定‘山顶’后根据递增或递减判断

```Golang

func validMountainArray(A []int) bool {
    aLen := len(A)
    top := -1
    for i := 0; i < aLen-1 ; i++ {
        if A[i + 1] <= A[i] {
            top = i
            break
        }
    }
    if top <= 0 || top == aLen -1 {
        return false
    }
    for i := 0; i < aLen ; i++ {
        if i < top &&  A[i+1] <= A[i] {
            return false
        }
        if i >= top && i < aLen -1 && (A[i+1] >= A[i]) {
            return false
        }
    }
    return true
}
```

### date 2020-11-4
#### [57. 插入区间](https://leetcode-cn.com/problems/insert-interval/)

```
给出一个无重叠的 ，按照区间起始端点排序的区间列表。

在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

 

示例 1：

输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]
示例 2：

输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。

```
> 分析：把插入区间依次比较，比区间小的可以直接插入左边，大的插入右边，有交集的取交集再比较

```Golang
func insert(intervals [][]int, newInterval []int) [][]int {
    ret := [][]int{}
    intervalsLen := len(intervals)
    if intervalsLen == 0 {
        return [][]int{newInterval}
    }
    for _,item := range intervals {
        if newInterval[0] <= item[1] && newInterval[1] >= item[0] {
            //有交集
            min := item[0]
            if newInterval[0] < item[0] {
                min = newInterval[0]
            }
            max := item[1]
            if newInterval[1] > item[1] {
                max = newInterval[1]
            }
            newInterval = []int{min, max}
        } else {
            if newInterval[1] < item[0] {
                ret = append(ret, newInterval)
                newInterval = item
            }
            if newInterval[0] > item[1] {
                ret = append(ret, item)
            }
        }
    }
    ret = append(ret, newInterval)
    return ret
}
```
