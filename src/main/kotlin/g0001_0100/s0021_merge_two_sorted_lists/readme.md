[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 21\. Merge Two Sorted Lists

Easy

Merge two sorted linked lists and return it as a **sorted** list. The list should be made by splicing together the nodes of the first two lists.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

**Input:** l1 = [1,2,4], l2 = [1,3,4]

**Output:** [1,1,2,3,4,4]

**Example 2:**

**Input:** l1 = [], l2 = []

**Output:** []

**Example 3:**

**Input:** l1 = [], l2 = [0]

**Output:** [0]

**Constraints:**

*   The number of nodes in both lists is in the range `[0, 50]`.
*   `-100 <= Node.val <= 100`
*   Both `l1` and `l2` are sorted in **non-decreasing** order.

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
    fun mergeTwoLists(list1: ListNode?, list2: ListNode?): ListNode? {
        var l1 = list1
        var l2 = list2
        var list: ListNode? = ListNode(-1)
        val head = list
        while (l1 != null || l2 != null) {
            if (l1 != null && l2 != null) {
                if (l1.`val` <= l2.`val`) {
                    list!!.next = ListNode(l1.`val`)
                    l1 = l1.next
                } else {
                    list!!.next = ListNode(l2.`val`)
                    l2 = l2.next
                }
            } else if (l1 != null) {
                list!!.next = ListNode(l1.`val`)
                l1 = l1.next
            } else {
                list!!.next = ListNode(l2!!.`val`)
                l2 = l2.next
            }
            list = list.next
        }
        return head!!.next
    }
}
```