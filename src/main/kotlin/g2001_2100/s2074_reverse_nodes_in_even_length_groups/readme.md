[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2074\. Reverse Nodes in Even Length Groups

Medium

You are given the `head` of a linked list.

The nodes in the linked list are **sequentially** assigned to **non-empty** groups whose lengths form the sequence of the natural numbers (`1, 2, 3, 4, ...`). The **length** of a group is the number of nodes assigned to it. In other words,

*   The <code>1<sup>st</sup></code> node is assigned to the first group.
*   The <code>2<sup>nd</sup></code> and the <code>3<sup>rd</sup></code> nodes are assigned to the second group.
*   The <code>4<sup>th</sup></code>, <code>5<sup>th</sup></code>, and <code>6<sup>th</sup></code> nodes are assigned to the third group, and so on.

Note that the length of the last group may be less than or equal to `1 + the length of the second to last group`.

**Reverse** the nodes in each group with an **even** length, and return _the_ `head` _of the modified linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/10/25/eg1.png)

**Input:** head = [5,2,6,3,9,1,7,3,8,4]

**Output:** [5,6,2,3,9,1,4,8,3,7]

**Explanation:**

- The length of the first group is 1, which is odd, hence no reversal occurs.

- The length of the second group is 2, which is even, hence the nodes are reversed.

- The length of the third group is 3, which is odd, hence no reversal occurs.

- The length of the last group is 4, which is even, hence the nodes are reversed. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/10/25/eg2.png)

**Input:** head = [1,1,0,6]

**Output:** [1,0,1,6]

**Explanation:**

- The length of the first group is 1. No reversal occurs.

- The length of the second group is 2. The nodes are reversed.

- The length of the last group is 1. No reversal occurs. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/11/17/ex3.png)

**Input:** head = [1,1,0,6,5]

**Output:** [1,0,1,5,6]

**Explanation:**

- The length of the first group is 1. No reversal occurs.

- The length of the second group is 2. The nodes are reversed.

- The length of the last group is 2. The nodes are reversed. 

**Constraints:**

*   The number of nodes in the list is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>0 <= Node.val <= 10<sup>5</sup></code>

## Solution

```kotlin
import com_github_leetcode.ListNode

/*
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun reverseEvenLengthGroups(head: ListNode?): ListNode? {
        var cnt = 1
        var currGroupCnt = 0
        var currNode = head
        var s = currNode
        while (currNode != null) {
            while (currGroupCnt++ < cnt && currNode != null) {
                val isEven = currGroupCnt % 2 == 0
                val isLastNodeInGroup = currGroupCnt == cnt || currNode.next == null
                if (isEven && isLastNodeInGroup) {
                    val end = currNode.next
                    val afterStart = s?.next
                    currNode = afterStart
                    var prev = s?.next
                    var curr = prev?.next
                    // First node of the group should link to 'start' of the next group exclusive
                    afterStart?.next = end
                    // Reverse this group, prev - curr - temp algorithm:
                    // curr will point to prev, new prev is curr, while curr shifts forward via tmp (:
                    while (curr != null && curr != end) {
                        val tmp = curr.next
                        curr.next = prev
                        prev = curr
                        curr = tmp
                    }
                    // Last node of the prev group should link to new first node of this one
                    s?.next = prev
                    s = afterStart
                } else if (!isEven && isLastNodeInGroup) {
                    s = currNode
                }
                currNode = currNode?.next
            }
            cnt++
            currGroupCnt = 0
        }
        return head
    }
}
```