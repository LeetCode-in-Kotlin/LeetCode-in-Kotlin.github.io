[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 24\. Swap Nodes in Pairs

Medium

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

**Input:** head = [1,2,3,4]

**Output:** [2,1,4,3]

**Example 2:**

**Input:** head = []

**Output:** []

**Example 3:**

**Input:** head = [1]

**Output:** [1]

**Constraints:**

*   The number of nodes in the list is in the range `[0, 100]`.
*   `0 <= Node.val <= 100`

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
    fun swapPairs(head: ListNode?): ListNode? {
        if (head == null) {
            return null
        }
        val len = getLength(head)
        return reverse(head, len)
    }

    private fun getLength(currLocal: ListNode): Int {
        var curr: ListNode? = currLocal
        var cnt = 0
        while (curr != null) {
            cnt++
            curr = curr.next
        }
        return cnt
    }

    // Recursive function to reverse in groups
    private fun reverse(head: ListNode?, len: Int): ListNode? {
        // base case
        if (len < 2) {
            return head
        }
        var curr = head
        var prev: ListNode? = null
        var next: ListNode?
        for (i in 0..1) {
            // reverse linked list code
            next = curr!!.next
            curr.next = prev
            prev = curr
            curr = next
        }
        head!!.next = reverse(curr, len - 2)
        return prev
    }
}
```