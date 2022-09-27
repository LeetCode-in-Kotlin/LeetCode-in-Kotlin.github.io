[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 114\. Flatten Binary Tree to Linked List

Medium

Given the `root` of a binary tree, flatten the tree into a "linked list":

*   The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
*   The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

**Input:** root = [1,2,5,3,4,null,6]

**Output:** [1,null,2,null,3,null,4,null,5,null,6]

**Example 2:**

**Input:** root = []

**Output:** []

**Example 3:**

**Input:** root = [0]

**Output:** [0]

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 2000]`.
*   `-100 <= Node.val <= 100`

**Follow up:** Can you flatten the tree in-place (with `O(1)` extra space)?

## Solution

```kotlin
import com_github_leetcode.TreeNode

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
    fun flatten(root: TreeNode?) {
        if (root != null) {
            findTail(root)
        }
    }

    private fun findTail(root: TreeNode): TreeNode {
        val left: TreeNode? = root.left
        val right: TreeNode? = root.right
        val tail: TreeNode
        // find the tail of left subtree, tail means the most left leaf
        if (left != null) {
            tail = findTail(left)
            // stitch the right subtree below the tail
            root.left = null
            root.right = left
            tail.right = right
        } else {
            tail = root
        }
        // find tail of the right subtree
        return if (tail.right == null) {
            tail
        } else {
            findTail(tail.right!!)
        }
    }
}
```