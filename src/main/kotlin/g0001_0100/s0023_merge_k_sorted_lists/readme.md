[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 23\. Merge k Sorted Lists

Hard

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

**Input:** lists = \[\[1,4,5],[1,3,4],[2,6]]

**Output:** [1,1,2,3,4,4,5,6]

**Explanation:** The linked-lists are: [ 1->4->5, 1->3->4, 2->6 ] merging them into one sorted list: 1->1->2->3->4->4->5->6

**Example 2:**

**Input:** lists = []

**Output:** []

**Example 3:**

**Input:** lists = \[\[]]

**Output:** []

**Constraints:**

*   `k == lists.length`
*   `0 <= k <= 10^4`
*   `0 <= lists[i].length <= 500`
*   `-10^4 <= lists[i][j] <= 10^4`
*   `lists[i]` is sorted in **ascending order**.
*   The sum of `lists[i].length` won't exceed `10^4`.

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
    fun mergeKLists(lists: Array<ListNode>): ListNode? {
        return if (lists.isEmpty()) {
            null
        } else mergeKLists(lists, 0, lists.size)
    }

    private fun mergeKLists(lists: Array<ListNode>, leftIndex: Int, rightIndex: Int): ListNode? {
        return if (rightIndex > leftIndex + 1) {
            val mid = (leftIndex + rightIndex) / 2
            val left = mergeKLists(lists, leftIndex, mid)
            val right = mergeKLists(lists, mid, rightIndex)
            mergeTwoLists(left, right)
        } else {
            lists[leftIndex]
        }
    }

    private fun mergeTwoLists(leftLocal: ListNode?, rightLocal: ListNode?): ListNode? {
        var left = leftLocal
        var right = rightLocal
        if (left == null) {
            return right
        }
        if (right == null) {
            return left
        }
        val res: ListNode
        if (left.`val` <= right.`val`) {
            res = left
            left = left.next
        } else {
            res = right
            right = right.next
        }
        var node: ListNode? = res
        while (left != null || right != null) {
            if (left == null) {
                node!!.next = right
                right = right!!.next
            } else if (right == null) {
                node!!.next = left
                left = left.next
            } else {
                if (left.`val` <= right.`val`) {
                    node!!.next = left
                    left = left.next
                } else {
                    node!!.next = right
                    right = right.next
                }
            }
            node = node.next
        }
        return res
    }
}
```