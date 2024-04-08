# 打家劫舍 III (House Robber III)

> leetcode 337题

### 题目

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。

除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 `root` 。返回 **在不触动警报的情况下** ，小偷能够盗取的最高金额 。

**示例 1:**

```
  3
 / \ 
2   3
 \   \ 
  3   1

输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

**示例 2:**

```
    3
   / \ 
  4   5
 / \   \ 
1   3   1

输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

**提示：**

- 树的节点数在 `[1, 10^4]` 范围内
- `0 <= Node.val <= 10^4`

### 思路

与`打家劫舍1`和`打家劫舍2`不同的是数据结构变成了二叉树，树的父子节点不能同时打劫。

- 打劫根节点，就不能打劫子节点，需要打劫孙节点；
- 不打劫根节点，就打劫子节点，即左右子节点；
- 比较两者，取较大值。

使用递归实现。

为了避免重复计算，对计算过的值缓存，使用时直接取。

```python
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def __init__(self):
        self.cache = {}

    def rob(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
        if root in self.cache:
            return self.cache[root]
        res1 = root.val
        if root.left:
            res1 += self.rob(root.left.left) + self.rob(root.left.right)
        if root.right:
            res1 += self.rob(root.right.left) + self.rob(root.right.right)

        res2 = self.rob(root.left) + self.rob(root.right)

        res3 = max(res1, res2)
        self.cache[root] = res3
        return res3
```

### 补充

将列表转成树的代码。

```python
# 左子树索引
left = lambda x: x*2+1
# 右子树索引
right = lambda x: x*2+2

def recursion(node: TreeNode, d, index):
    length = len(d)

    l_id = left(index)
    if l_id < length and d[l_id] is not None:
        node.left = TreeNode(d[l_id])
        recursion(node.left, d, l_id)

    r_id = right(index)
    if r_id < length and d[r_id] is not None:
        node.right = TreeNode(d[r_id])
        recursion(node.right, d, r_id)

def list_to_tree(d):
    node = TreeNode(d[0])
    recursion(node, d, 0)
    return node

if __name__ == '__main__':
    a = [3, 2, 3, None, 3, None, 1]
    # a = [3, 4, 5, 1, 3, None, 1]
    node = list_to_tree(a)
```

