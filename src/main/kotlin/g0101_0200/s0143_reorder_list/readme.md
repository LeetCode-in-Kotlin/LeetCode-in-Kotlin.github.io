[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 143\. Reorder List

Medium

You are given the head of a singly linked-list. The list can be represented as:

L<sub>0</sub> → L<sub>1</sub> → … → L<sub>n - 1</sub> → L<sub>n</sub>

_Reorder the list to be on the following form:_

L<sub>0</sub> → L<sub>n</sub> → L<sub>1</sub> → L<sub>n - 1</sub> → L<sub>2</sub> → L<sub>n - 2</sub> → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

**Input:** head = [1,2,3,4]

**Output:** [1,4,2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

**Input:** head = [1,2,3,4,5]

**Output:** [1,5,2,4,3]

**Constraints:**

*   The number of nodes in the list is in the range <code>[1, 5 * 10<sup>4</sup>]</code>.
*   `1 <= Node.val <= 1000`

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
    private var forward: ListNode? = null
    fun reorderList(head: ListNode?) {
        forward = head
        dfs(head)
    }

    private fun dfs(node: ListNode?) {
        if (node != null) {
            dfs(node.next)
            if (forward != null) {
                val next = forward!!.next
                // even case: forward.next coincide with node
                if (next == node) {
                    node.next = null
                } else {
                    node.next = next
                }
                forward!!.next = node
                forward = node.next
            }
            // odd case: forward coincide with node
            if (forward == node) {
                forward!!.next = null
                forward = forward!!.next
            }
        }
    }
}
```