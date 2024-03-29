[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2095\. Delete the Middle Node of a Linked List

Medium

You are given the `head` of a linked list. **Delete** the **middle node**, and return _the_ `head` _of the modified linked list_.

The **middle node** of a linked list of size `n` is the <code>⌊n / 2⌋<sup>th</sup></code> node from the **start** using **0-based indexing**, where `⌊x⌋` denotes the largest integer less than or equal to `x`.

*   For `n` = `1`, `2`, `3`, `4`, and `5`, the middle nodes are `0`, `1`, `1`, `2`, and `2`, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/16/eg1drawio.png)

**Input:** head = [1,3,4,7,1,2,6]

**Output:** [1,3,4,1,2,6]

**Explanation:**

The above figure represents the given linked list.

The indices of the nodes are written below. Since n = 7, node 3 with value 7 is the middle node, which is marked in red.

We return the new list after removing this node. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/16/eg2drawio.png)

**Input:** head = [1,2,3,4]

**Output:** [1,2,4]

**Explanation:**

The above figure represents the given linked list.

For n = 4, node 2 with value 3 is the middle node, which is marked in red. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/11/16/eg3drawio.png)

**Input:** head = [2,1]

**Output:** [2]

**Explanation:**

The above figure represents the given linked list.

For n = 2, node 1 with value 1 is the middle node, which is marked in red.

Node 0 with value 2 is the only node remaining after removing node 1.

**Constraints:**

*   The number of nodes in the list is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>5</sup></code>

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
    fun deleteMiddle(head: ListNode?): ListNode? {
        var slow = head
        var fast = head
        var prev: ListNode? = null
        while (fast?.next != null) {
            prev = slow

            slow = slow?.next
            fast = fast.next?.next
        }
        if (slow == head) {
            return null
        }
        prev?.next = slow?.next
        return head
    }
}
```