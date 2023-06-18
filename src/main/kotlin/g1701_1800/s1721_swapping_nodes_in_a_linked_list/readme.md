[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1721\. Swapping Nodes in a Linked List

Medium

You are given the `head` of a linked list, and an integer `k`.

Return _the head of the linked list after **swapping** the values of the_ <code>k<sup>th</sup></code> _node from the beginning and the_ <code>k<sup>th</sup></code> _node from the end (the list is **1-indexed**)._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/21/linked1.jpg)

**Input:** head = [1,2,3,4,5], k = 2

**Output:** [1,4,3,2,5]

**Example 2:**

**Input:** head = [7,9,6,6,7,8,3,0,9,5], k = 5

**Output:** [7,9,6,6,8,7,3,0,9,5]

**Constraints:**

*   The number of nodes in the list is `n`.
*   <code>1 <= k <= n <= 10<sup>5</sup></code>
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
@Suppress("NAME_SHADOWING")
class Solution {
    fun swapNodes(head: ListNode?, k: Int): ListNode? {
        var k: Int = k
        var beg: ListNode? = null
        var end: ListNode? = null
        var node = head
        while (node != null) {
            k--
            if (k == 0) {
                beg = node
                end = head
            } else if (end != null) {
                end = end.next
            }
            node = node.next
        }
        if (beg != null) {
            val tem = beg.`val`
            beg.`val` = end!!.`val`
            end.`val` = tem
        }
        return head
    }
}
```