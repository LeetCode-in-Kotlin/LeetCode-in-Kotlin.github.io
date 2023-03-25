[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 817\. Linked List Components

Medium

You are given the `head` of a linked list containing unique integer values and an integer array `nums` that is a subset of the linked list values.

Return _the number of connected components in_ `nums` _where two values are connected if they appear **consecutively** in the linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom1.jpg)

**Input:** head = [0,1,2,3], nums = [0,1,3]

**Output:** 2

**Explanation:** 0 and 1 are connected, so [0, 1] and [3] are the two connected components.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom2.jpg)

**Input:** head = [0,1,2,3,4], nums = [0,3,1,4]

**Output:** 2

**Explanation:** 0 and 1 are connected, 3 and 4 are connected, so [0, 1] and [3, 4] are the two connected components.

**Constraints:**

*   The number of nodes in the linked list is `n`.
*   <code>1 <= n <= 10<sup>4</sup></code>
*   `0 <= Node.val < n`
*   All the values `Node.val` are **unique**.
*   `1 <= nums.length <= n`
*   `0 <= nums[i] < n`
*   All the values of `nums` are **unique**.

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

@Suppress("NAME_SHADOWING")
class Solution {
    fun numComponents(head: ListNode?, nums: IntArray): Int {
        var head = head
        val set: HashSet<Int> = HashSet()
        for (i in nums) {
            set.add(i)
        }
        var result = 0
        while (head != null) {
            if (set.contains(head.`val`)) {
                while (head != null && set.contains(head.`val`)) {
                    head = head.next
                }
                result++
            } else {
                head = head.next
            }
        }
        return result
    }
}
```