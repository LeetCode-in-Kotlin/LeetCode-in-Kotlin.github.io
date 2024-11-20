[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 222\. Count Complete Tree Nodes

Medium

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and <code>2<sup>h</sup></code> nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

**Input:** root = [1,2,3,4,5,6]

**Output:** 6

**Example 2:**

**Input:** root = []

**Output:** 0

**Example 3:**

**Input:** root = [1]

**Output:** 1

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 5 * 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 5 * 10<sup>4</sup></code>
*   The tree is guaranteed to be **complete**.

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
    fun countNodes(root: TreeNode?): Int {
        if (root == null) {
            return 0
        }
        val leftHeight = leftHeight(root)
        val rightHeight = rightHeight(root)
        // case 1: When Height(Left sub-tree) = Height(right sub-tree) 2^h - 1
        return if (leftHeight == rightHeight) {
            (1 shl leftHeight) - 1
        } else {
            1 + countNodes(root.left) + countNodes(root.right)
        }
    }

    private fun leftHeight(root: TreeNode?): Int {
        return if (root == null) {
            0
        } else {
            1 + leftHeight(root.left)
        }
    }

    private fun rightHeight(root: TreeNode?): Int {
        return if (root == null) {
            0
        } else {
            1 + rightHeight(root.right)
        }
    }
}
```