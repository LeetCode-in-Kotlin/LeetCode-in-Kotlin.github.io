[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2181\. Merge Nodes in Between Zeros

Medium

You are given the `head` of a linked list, which contains a series of integers **separated** by `0`'s. The **beginning** and **end** of the linked list will have `Node.val == 0`.

For **every** two consecutive `0`'s, **merge** all the nodes lying in between them into a single node whose value is the **sum** of all the merged nodes. The modified list should not contain any `0`'s.

Return _the_ `head` _of the modified linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/02/02/ex1-1.png)

**Input:** head = [0,3,1,0,4,5,2,0]

**Output:** [4,11]

**Explanation:**

The above figure represents the given linked list. The modified list contains

- The sum of the nodes marked in green: 3 + 1 = 4.

- The sum of the nodes marked in red: 4 + 5 + 2 = 11. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/02/02/ex2-1.png)

**Input:** head = [0,1,0,3,0,2,2,0]

**Output:** [1,3,4]

**Explanation:**

The above figure represents the given linked list. The modified list contains

- The sum of the nodes marked in green: 1 = 1.

- The sum of the nodes marked in red: 3 = 3.

- The sum of the nodes marked in yellow: 2 + 2 = 4. 

**Constraints:**

*   The number of nodes in the list is in the range <code>[3, 2 * 10<sup>5</sup>]</code>.
*   `0 <= Node.val <= 1000`
*   There are **no** two consecutive nodes with `Node.val == 0`.
*   The **beginning** and **end** of the linked list have `Node.val == 0`.

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
    fun mergeNodes(head: ListNode): ListNode? {
        var temp = head.next
        var slow = head
        var sum = 0
        var fast = temp
        while (temp != null) {
            if (temp.`val` == 0) {
                temp.`val` = sum
                sum = 0
                slow.next = fast!!.next
                slow = temp
                fast = fast.next
            } else {
                sum += temp.`val`
                fast = temp
            }
            temp = temp.next
        }
        return head.next
    }
}
```