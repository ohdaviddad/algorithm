# 打家劫舍 (House Robber)

> leetcode 198题

### 题目

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

### 思路

当输入`[1]`，只有一个房屋，直接偷窃这个。

当输入`[1, 2]`，偷两个房屋中金额多的那个，即`2`。

当输入`[1, 2, 3]`，当小偷在第3个房间，这时就要判断是否偷窃当前房屋的现金：

- 如果偷，总金额就是，前一个不相邻的房屋的总金额与当前房屋的现金之和，即走到第1个房子偷到的总金额与当前房子现金之和，`1+3=4`；
- 如果不偷，总金额就是，前一个相邻房屋的总金额，即走到第2个房子偷到的总金额，`2`；
- 比较两个金额，取较大的那个，即`4`。

当输入`[1, 2, 3, 1]`，前3步与上面相同，当小偷在第4个房间，判断是否偷窃当前房间的现金：

- 如果偷，总金额就是，前一个不相邻的房屋的总金额与当前房屋的现金之和，即走到第2个房子偷到的总金额与当前房子现金之和，`2+1=3`；
- 如果不偷，总金额就是，前一个相邻房屋的总金额，即走到第3个房子偷到的总金额，`4`；
- 比较两个金额，取较大的那个，即`4`。

以此类推，走到第n个房子时：

```
dp[n] = max(dp[n-2]+values[n], dp[n-1])
```

由此可以写出代码：

```python
def rob(nums):
    if len(nums) == 1:
        return nums[0]
    if len(nums) == 2:
        return max(nums[0], nums[1])
    dp = [0] * len(nums)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    for i in range(2, len(nums)):
        dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])
    return dp[-1]
```
