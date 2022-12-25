[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 445\. Add Two Numbers II

Medium

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/sumii-linked-list.jpg)

**Input:** l1 = [7,2,4,3], l2 = [5,6,4]

**Output:** [7,8,0,7]

**Example 2:**

**Input:** l1 = [2,4,3], l2 = [5,6,4]

**Output:** [8,0,7]

**Example 3:**

**Input:** l1 = [0], l2 = [0]

**Output:** [0]

**Constraints:**

*   The number of nodes in each linked list is in the range `[1, 100]`.
*   `0 <= Node.val <= 9`
*   It is guaranteed that the list represents a number that does not have leading zeros.

**Follow up:** Could you solve it without reversing the input lists?

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
    private fun reverse(head: ListNode?): ListNode? {
        if (head == null || head.next == null) {
            return head
        }
        var prev: ListNode? = null
        var curr: ListNode = head
        var next = head.next
        while (next != null) {
            curr.next = prev
            prev = curr
            curr = next
            next = next.next
        }
        curr.next = prev
        return curr
    }

    fun addTwoNumbers(l1: ListNode?, l2: ListNode?): ListNode? {
        var l1 = l1
        var l2 = l2
        l1 = reverse(l1)
        l2 = reverse(l2)
        var res: ListNode? = ListNode()
        val head = res
        var carry = 0
        while (l1 != null || l2 != null) {
            var val1: Int
            var val2: Int
            if (l1 == null) {
                val1 = 0
            } else {
                val1 = l1.`val`
                l1 = l1.next
            }
            if (l2 == null) {
                val2 = 0
            } else {
                val2 = l2.`val`
                l2 = l2.next
            }
            var data = val1 + val2 + carry
            if (data > 9) {
                carry = data / 10
                data = data % 10
            } else {
                carry = 0
            }
            res!!.next = ListNode(data)
            res = res.next
        }
        if (carry != 0) {
            res!!.next = ListNode(carry)
        }
        return reverse(head!!.next)
    }
}
```