[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 234\. Palindrome Linked List

Easy

Given the `head` of a singly linked list, return `true` _if it is a palindrome or_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

**Input:** head = [1,2,2,1]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

**Input:** head = [1,2]

**Output:** false

**Constraints:**

*   The number of nodes in the list is in the range <code>[1, 10<sup>5</sup>]</code>.
*   `0 <= Node.val <= 9`

**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

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
    fun isPalindrome(head: ListNode?): Boolean {
        var head = head
        var len = 0
        var right = head
        // Calculate the length
        while (right != null) {
            right = right.next
            len++
        }
        // Reverse the right half of the list
        len = len / 2
        right = head
        for (i in 0 until len) {
            right = right!!.next
        }
        var prev: ListNode? = null
        while (right != null) {
            val next = right.next
            right.next = prev
            prev = right
            right = next
        }
        // Compare left half and right half
        for (i in 0 until len) {
            if (prev != null && head!!.`val` == prev.`val`) {
                head = head.next
                prev = prev.next
            } else {
                return false
            }
        }
        return true
    }
}
```