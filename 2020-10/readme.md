### date 2020-10-27

#### 1480. 一维数组的动态和
> 给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。请返回 nums 的动态和。

> 输入：nums = [1,2,3,4]
>输出：[1,3,6,10]
>解释：动态和计算过程为 [1, 1+2, 1+2+3, 1+2+3+4] 。

> 输入：nums = [1,1,1,1,1]
输出：[1,2,3,4,5]
解释：动态和计算过程为 [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1] 。


```Golang

func runningSum(nums []int) []int {
    numsLen := len(nums)
    for i := 1; i < numsLen; i++ {
        nums[i] += num[i-1]
    }
    return nums
}
```

### date 2020-10-28

#### 1207. 独一无二的出现次数
> 给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数。如果每个数的出现次数都是独一无二的，就返回 true；否则返回 false

> 输入：arr = [1,2,2,1,1,3]
> 输出：true 
> 解释：在该数组中，1 出现了 3 次，2 出现了 2 次，3 只出现了 1 次。没有两个数的出现次数相同。

> 输入：arr = [1,2]
> 输出：false

```Golang
func uniqueOccurrences(arr []int) bool {
    cnt := make(map[int]int)
    times := make(map[int]struct{})
    for _, i := range arr {
        cnt[i]++
    }
    for _, j := range cnt {
        times[j] = struct{}{}
    }
    return len(cnt) == len(times)
}
```
> 分析：构建hash表计算各值得次数，再构建次数的hash，如果值的次数hash表长度和次数hash表长度不等就是存在重复的
