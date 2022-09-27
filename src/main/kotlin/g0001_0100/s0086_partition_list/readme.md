[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 86\. Partition List

Medium

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

**Input:** head = [1,4,3,2,5,2], x = 3

**Output:** [1,2,2,4,3,5]

**Example 2:**

**Input:** head = [2,1], x = 2

**Output:** [1,2]

**Constraints:**

*   The number of nodes in the list is in the range `[0, 200]`.
*   `-100 <= Node.val <= 100`
*   `-200 <= x <= 200`

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
@Suppress("NAME_SHADOWING")
class Solution {
    fun partition(head: ListNode?, x: Int): ListNode? {
        var head = head
        var nHead: ListNode? = ListNode(0)
        var nTail: ListNode? = ListNode(0)
        val ptr = nTail
        val temp = nHead
        while (head != null) {
            val nNext = head.next
            if (head.`val` < x) {
                nHead!!.next = head
                nHead = nHead.next
            } else {
                nTail!!.next = head
                nTail = nTail.next
            }
            head = nNext
        }
        nTail!!.next = null
        nHead!!.next = ptr!!.next
        return temp!!.next
    }
}
```