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

### date 2020-0-29
#### [129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)
> 给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。
例如，从根到叶子节点路径 1->2->3 代表数字 123。
计算从根到叶子节点生成的所有数字之和。
```
输入: [1,2,3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
```

```
输入: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
输出: 1026
解释:
从根到叶子节点路径 4->9->5 代表数字 495.
从根到叶子节点路径 4->9->1 代表数字 491.
从根到叶子节点路径 4->0 代表数字 40.
因此，数字总和 = 495 + 491 + 40 = 1026.
```

> 分析：二叉树的每条从根节点到叶子节点的路径都代表一个数字。其实，每个节点都对应一个数字，等于其父节点对应的数字乘以 1010 再加上该节点的值（这里假设根节点的父节点对应的数字是 00）。只要计算出每个叶子节点对应的数字，然后计算所有叶子节点对应的数字之和，即可得到结果
```Golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func sumNumbers(root *TreeNode) int {
    return get(root, 0)
}

func get(root *TreeNode, total int) int {
    if root == nil {
        return 0
    }
    if root.Left == nil && root.Right == nil {
        return root.Val + total * 10
    }
    return get(root.Left, root.Val + total * 10) + get(root.Right, root.Val + total * 10)
}
```

### date 2020-10-30
#### [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

>给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。
网格中的格子水平和垂直方向相连（对角线方向不相连）。
整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。
岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

```
输入:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

输出: 16

解释: 它的周长是下面图片中的 16 个黄色的边：

```
![岛屿的周长](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/island.png)

> 分析：每个格子条边如果是跟水域接触或为边界就不算周长，所以可以遍历格子，判断四条边相邻的状态，如果是水域或边界则加1.

```
func islandPerimeter(grid [][]int) int {
    sum := 0
    m := len(grid)
    for i, item := range grid {
        for j, v := range item {
            n := len(item)
            if v > 0 {
                if j-1 < 0 || item[j-1] == 0 {
                    sum++;
                }
                if i-1 <0 || grid[i-1][j] == 0 {
                    sum++;
                }
                if j+1 >= n || item[j+1] == 0 {
                    sum++;
                }
                if i+1 >= m || grid[i+1][j] == 0 {
                    sum++;
                }
            }
        }
    }
    return sum
}
```
### date 2020-10-37
#### [381. O(1) 时间插入、删除和获取随机元素 - 允许重复](https://leetcode-cn.com/problems/insert-delete-getrandom-o1-duplicates-allowed/)

```
设计一个支持在平均 时间复杂度 O(1) 下， 执行以下操作的数据结构。

注意: 允许出现重复元素。

insert(val)：向集合中插入元素 val。
remove(val)：当 val 存在时，从集合中移除一个 val。
getRandom：从现有集合中随机获取一个元素。每个元素被返回的概率应该与其在集合中的数量呈线性相关。

示例

// 初始化一个空的集合。
RandomizedCollection collection = new RandomizedCollection();

// 向集合中插入 1 。返回 true 表示集合不包含 1 。
collection.insert(1);

// 向集合中插入另一个 1 。返回 false 表示集合包含 1 。集合现在包含 [1,1] 。
collection.insert(1);

// 向集合中插入 2 ，返回 true 。集合现在包含 [1,1,2] 。
collection.insert(2);

// getRandom 应当有 2/3 的概率返回 1 ，1/3 的概率返回 2 。
collection.getRandom();

// 从集合中删除 1 ，返回 true 。集合现在包含 [1,2] 。
collection.remove(1);

// getRandom 应有相同概率返回 1 和 2 。
collection.getRandom();

```

> 分析：O(1)时间可以考虑到哈希表，依据题目可以把数据存储到hash表，insert解决；建立map作为值跟位置的映射关系，因为可以重复，可以建立二维 `map[int]map[int]struct{}  `，remove时把值移除，把末尾的值剪切到移除的位置保证其他值的映射关系不会改变。注意点：剪切后映射为空的问题；移除末尾元素问题；

```Golang
type RandomizedCollection struct {
    data []int
    dataMap map[int]map[int]struct{}
}


/** Initialize your data structure here. */
func Constructor() RandomizedCollection {
    return RandomizedCollection{
        dataMap: map[int]map[int]struct{}{},
    }
}


/** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
func (this *RandomizedCollection) Insert(val int) bool {
    _, ok := this.dataMap[val]
    if !ok {
        this.dataMap[val] = map[int]struct{}{}
    }
    this.data = append(this.data, val)
    index := len(this.data) -1
    this.dataMap[val][index] = struct{}{}
    return !ok
}


/** Removes a value from the collection. Returns true if the collection contained the specified element. */
func (this *RandomizedCollection) Remove(val int) bool {
    value, ok  := this.dataMap[val]
    if ok {
        //把最后的元素填充移除的元素
        index := 0
        for k,_ := range value {
            index = k
        }
        maxIndex := len(this.data)-1 //最后一个元素下标
        maxVal := this.data[maxIndex] //最后一个元素值
        //删除data
        if maxIndex > 0 {
            this.data[index] = maxVal
            this.data = this.data[0:maxIndex]
        } else {
            this.data = []int{}
        }
        
        //删除map中的元素
        delete(this.dataMap[val], index)
        if len(this.dataMap[val]) == 0 {
            delete(this.dataMap, val)
        }
        if index != maxIndex {
            //删除最后一个元素的map
            delete(this.dataMap[maxVal], maxIndex)
            //最后一个元素map添加到删除的map
            this.dataMap[maxVal][index] = struct{}{}
        }
        
    }
    return ok
}


/** Get a random element from the collection. */
func (this *RandomizedCollection) GetRandom() int {
    rand.Seed(time.Now().UnixNano())
    x := rand.Intn(len(this.data))
    return this.data[x]
}


/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Insert(val);
 * param_2 := obj.Remove(val);
 * param_3 := obj.GetRandom();
 */
```
