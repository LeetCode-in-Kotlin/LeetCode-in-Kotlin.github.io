[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1372\. Longest ZigZag Path in a Binary Tree

Medium

You are given the `root` of a binary tree.

A ZigZag path for a binary tree is defined as follow:

*   Choose **any** node in the binary tree and a direction (right or left).
*   If the current direction is right, move to the right child of the current node; otherwise, move to the left child.
*   Change the direction from right to left or from left to right.
*   Repeat the second and third steps until you can't move in the tree.

Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return _the longest **ZigZag** path contained in that tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)

**Input:** root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1,null,1]

**Output:** 3

**Explanation:** Longest ZigZag path in blue nodes (right -> left -> right).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/22/sample_2_1702.png)

**Input:** root = [1,1,1,null,1,null,null,1,1,null,1]

**Output:** 4

**Explanation:** Longest ZigZag path in blue nodes (left -> right -> left -> right).

**Example 3:**

**Input:** root = [1]

**Output:** 0

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 5 * 10<sup>4</sup>]</code>.
*   `1 <= Node.val <= 100`

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
    private var maxLength = 0

    fun longestZigZag(root: TreeNode?): Int {
        dfs(root, true)
        return maxLength
    }

    private fun dfs(root: TreeNode?, isLeft: Boolean): Int {
        if (root == null) {
            return 0
        }
        val left = dfs(root.left, false)
        val right = dfs(root.right, true)
        maxLength = Math.max(maxLength, left)
        maxLength = Math.max(maxLength, right)
        return 1 + if (isLeft) left else right
    }
}
```