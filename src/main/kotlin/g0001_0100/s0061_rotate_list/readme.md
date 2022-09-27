[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 61\. Rotate List

Medium

Given the `head` of a linked list, rotate the list to the right by `k` places.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)

**Input:** head = [1,2,3,4,5], k = 2

**Output:** [4,5,1,2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)

**Input:** head = [0,1,2], k = 4

**Output:** [2,0,1]

**Constraints:**

*   The number of nodes in the list is in the range `[0, 500]`.
*   `-100 <= Node.val <= 100`
*   <code>0 <= k <= 2 * 10<sup>9</sup></code>

## Solution

```kotlin
import com_github_leetcode.ListNode

/**
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun rotateRight(head: ListNode?, k: Int): ListNode? {
        if (head == null || k == 0) {
            return head
        }
        var tail = head
        // find the count and let tail points to last node
        var count = 1
        while (tail != null && tail.next != null) {
            count++
            tail = tail.next
        }
        // calculate number of times to rotate by count modulas
        val times = k % count
        if (times == 0) {
            return head
        }
        var temp = head
        // iterate and go to the K+1 th node from the end or count - K - 1 node from
        // start
        var i = 1
        while (i <= count - times - 1 && temp != null) {
            temp = temp.next
            i++
        }
        var newHead: ListNode? = null
        if (temp != null && tail != null) {
            newHead = temp.next
            temp.next = null
            tail.next = head
        }
        return newHead
    }
}
```