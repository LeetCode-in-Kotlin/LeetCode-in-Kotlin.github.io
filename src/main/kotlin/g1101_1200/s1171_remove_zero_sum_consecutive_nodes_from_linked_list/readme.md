[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1171\. Remove Zero Sum Consecutive Nodes from Linked List

Medium

Given the `head` of a linked list, we repeatedly delete consecutive sequences of nodes that sum to `0` until there are no such sequences.

After doing so, return the head of the final linked list. You may return any such answer.

(Note that in the examples below, all sequences are serializations of `ListNode` objects.)

**Example 1:**

**Input:** head = [1,2,-3,3,1]

**Output:** [3,1] **Note:** The answer [1,2,1] would also be accepted.

**Example 2:**

**Input:** head = [1,2,3,-3,4]

**Output:** [1,2,4]

**Example 3:**

**Input:** head = [1,2,3,-3,-2]

**Output:** [1]

**Constraints:**

*   The given linked list will contain between `1` and `1000` nodes.
*   Each node in the linked list has `-1000 <= node.val <= 1000`.

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
    fun removeZeroSumSublists(head: ListNode?): ListNode? {
        val pre = ListNode(-1)
        var curr: ListNode? = pre
        pre.next = head
        val map: MutableMap<Int, ListNode> = HashMap()
        var preSum = 0
        while (curr != null) {
            preSum += curr.`val`
            if (map.containsKey(preSum)) {
                curr = map.getValue(preSum).next
                var key = preSum + curr!!.`val`
                while (key != preSum) {
                    map.remove(key)
                    curr = curr!!.next
                    key += curr!!.`val`
                }
                map.getValue(preSum).next = curr.next
            } else {
                map[preSum] = curr
            }
            curr = curr.next
        }
        return pre.next
    }
}
```