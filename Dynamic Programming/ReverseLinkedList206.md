> leetcode 206题 反转链表

给你单链表的头节点 <code>head</code> ，请你反转链表，并返回反转后的链表。

<div class="original__bRMd"> 
 <div> 
  <p>&nbsp;</p> 
 </div>
</div>

<p><strong>示例 1：</strong></p> 
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg" style="width: 542px; height: 222px;" /> 

<pre>
<strong>输入：</strong>head = [1,2,3,4,5]
<strong>输出：</strong>[5,4,3,2,1]
</pre>

<p><strong>示例 2：</strong></p> 
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg" style="width: 182px; height: 222px;" /> 
<pre>
<strong>输入：</strong>head = [1,2]
<strong>输出：</strong>[2,1]
</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入：</strong>head = []
<strong>输出：</strong>[]
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul> 
 <li>链表中节点的数目范围是 <code>[0, 5000]</code></li> 
 <li><code>-5000 &lt;= Node.val &lt;= 5000</code></li> 
</ul>

<p>&nbsp;</p>

<p><strong>进阶：</strong>链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？</p>

<div><div>Related Topics</div><div><li>递归</li><li>链表</li></div></div><br><div><li>👍 3558</li><li>👎 0</li></div>

## 思路

- 双指针：遍历时改变 node 的指向，需要注意的就是保存 当前node.next指针，因为node.next如果指向prev时，就找不到了 node.next；所以需要先保存下来。例如 1->2->3, 当前 curr=2;如果让 curr -> 1，需要先将 3 保存下来；

```java
class Solution {
  public ListNode reverseList(ListNode head) {
    ListNode cur = head;
    ListNode pre = null;
    if (cur == null) {
      return null;
    }
    ListNode temp；
      while (cur != null) {
        temp = cur.next;
        cur.next = pre;
        pre = cur;
        cur = temp;
      }
    return pre;
  }
}
```

