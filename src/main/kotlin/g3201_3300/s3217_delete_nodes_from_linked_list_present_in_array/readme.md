[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3217\. Delete Nodes From Linked List Present in Array

Medium

You are given an array of integers `nums` and the `head` of a linked list. Return the `head` of the modified linked list after **removing** all nodes from the linked list that have a value that exists in `nums`.

**Example 1:**

**Input:** nums = [1,2,3], head = [1,2,3,4,5]

**Output:** [4,5]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample0.png)**

Remove the nodes with values 1, 2, and 3.

**Example 2:**

**Input:** nums = [1], head = [1,2,1,2,1,2]

**Output:** [2,2,2]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample1.png)

Remove the nodes with value 1.

**Example 3:**

**Input:** nums = [5], head = [1,2,3,4]

**Output:** [1,2,3,4]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample2.png)**

No node has value 5.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   All elements in `nums` are unique.
*   The number of nodes in the given list is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>5</sup></code>
*   The input is generated such that there is at least one node in the linked list that has a value not present in `nums`.

## Solution

```kotlin
import com_github_leetcode.ListNode
import kotlin.math.max

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
    fun modifiedList(nums: IntArray, head: ListNode?): ListNode? {
        var maxv = 0
        for (v in nums) {
            maxv = max(maxv, v)
        }
        val rem = BooleanArray(maxv + 1)
        for (v in nums) {
            rem[v] = true
        }
        val h = ListNode(0)
        var t = h
        var p = head
        while (p != null) {
            if (p.`val` > maxv || !rem[p.`val`]) {
                t.next = p
                t = p
            }
            p = p.next
        }
        t.next = null
        return h.next
    }
}
```