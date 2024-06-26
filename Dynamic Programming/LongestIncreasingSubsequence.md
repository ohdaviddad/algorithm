# 最长递增子序列 (Longest Increasing Subsequence)

> leetcode 300题

### 题目

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

**提示：**

- `1 <= nums.length <= 2500`
- `-10^4 <= nums[i] <= 10^4`

**进阶：**

- 你能将算法的时间复杂度降低到 `O(n log(n))` 吗?

### 思路

以数组`nums = [0,1,0,3,2,3]`为例。

`dp[i]`表示**必须**以数组`nums[i]`元素结尾的最大递增子序列的长度。

```
dp[0] = 1, [0]
dp[1] = 2, [0,1]
dp[2] = 1, [0]
因为在[0,1,0]中0和1都不小于0，所以只能以第二个0开始构成递增序列

dp[3] = 3, [0,1,3]
3比0和1都大，所以dp[3]可以是dp[0]+1=2,dp[1]+1=3,dp[2]+1=2
取最大值，dp[3] = dp[1]+1=3

dp[4] = 3, [0,1,2]
2比0和1大，所以dp[4]可以是dp[0]+1=2,dp[1]+1=3,dp[2]+1=2
取最大值，dp[4] = dp[1]+1=3

dp[5] = 4, [0,1,2,3]
3比0,1,2都大，所以dp[5]可以是dp[0]+1=2,dp[1]+1=3,dp[2]+1=2和dp[4]+1=4
取最大值，dp[5] = dp[4]+1=4
```

只要知道某个元素的为末尾的最长递增子序列，那么后面元素只要比该元素大，就可以构成一个新的递增子序列，长度就是在其基础上加1。

所以

```
长度为n的数组nums
以nums[i]结尾的最长递增子序列长度为dp[i]
j < i < n
当 nums[j] < nums[i]
dp[i] = max(dp[i], dp[j]+1)
```

由此，写出代码：

```python
from typing import List

class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1 for _ in range(len(nums))]
        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)

        return max(dp)
```

单个元素本身也是一个长度为1的递增子序列，所以先初始化一个长度与数组相同，值都为1的dp数组。

然后从数组第二个元素开始，即nums[1]开始；

每次从头开始，寻找以该元素结尾，最长的递增子序列。



