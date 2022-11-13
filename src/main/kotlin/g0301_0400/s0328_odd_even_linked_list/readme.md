[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 328\. Odd Even Linked List

Medium

Given the `head` of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return _the reordered list_.

The **first** node is considered **odd**, and the **second** node is **even**, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg)

**Input:** head = [1,2,3,4,5]

**Output:** [1,3,5,2,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)

**Input:** head = [2,1,3,5,6,4,7]

**Output:** [2,3,6,7,1,5,4]

**Constraints:**

*   The number of nodes in the linked list is in the range <code>[0, 10<sup>4</sup>]</code>.
*   <code>-10<sup>6</sup> <= Node.val <= 10<sup>6</sup></code>

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
    fun oddEvenList(head: ListNode?): ListNode? {
        val odd = ListNode(0)
        val even = ListNode(0)
        var oddPointer = odd
        var evenPointer = even
        var pointer = head
        var count = 0
        while (pointer != null) {
            if (count % 2 == 0) {
                oddPointer.next = pointer
                oddPointer = oddPointer.next!!
            } else {
                evenPointer.next = pointer
                evenPointer = evenPointer.next!!
            }
            val next = pointer.next
            pointer.next = null
            pointer = next
            count++
        }
        oddPointer.next = even.next
        return odd.next
    }
}
```