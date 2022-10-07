[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 109\. Convert Sorted List to Binary Search Tree

Medium

Given the `head` of a singly linked list where elements are **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of _every_ node never differ by more than 1.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

**Input:** head = [-10,-3,0,5,9]

**Output:** [0,-3,9,-10,null,5]

**Explanation:** One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.

**Example 2:**

**Input:** head = []

**Output:** []

**Constraints:**

*   The number of nodes in `head` is in the range <code>[0, 2 * 10<sup>4</sup>]</code>.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>

## Solution

```kotlin
import com_github_leetcode.ListNode
import com_github_leetcode.TreeNode

/*
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
/*
 * Example:
 * var ti = TreeNode(5)
 * var v = ti.`val`
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    fun sortedListToBST(head: ListNode?): TreeNode? {
        // Empty list -> empty tree / sub-tree
        if (head == null) {
            return null
        }
        // No next node -> this node will become leaf
        if (head.next == null) {
            val leaf = TreeNode(head.`val`)
            leaf.left = null
            leaf.right = null
            return leaf
        }
        var slow = head
        // Head-Start fast by 1 to get slow = mid -1
        var fast = head.next!!.next
        // Find the mid of list
        while (fast != null && fast.next != null) {
            slow = slow!!.next
            fast = fast.next!!.next
        }
        // slow.next ->  mid = our "root"
        val root = TreeNode(slow!!.next!!.`val`)
        // Right sub tree from mid - end
        root.right = sortedListToBST(slow.next!!.next)
        // Left sub tree from head - mid (chop slow.next)
        slow.next = null
        root.left = sortedListToBST(head)
        return root
    }
}
```