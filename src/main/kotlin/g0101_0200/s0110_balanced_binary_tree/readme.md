[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 110\. Balanced Binary Tree

Easy

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of _every_ node differ in height by no more than 1.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

**Input:** root = [1,2,2,3,3,null,null,4,4]

**Output:** false

**Example 3:**

**Input:** root = []

**Output:** true

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 5000]`.
*   <code>-10<sup>4</sup> <= Node.val <= 10<sup>4</sup></code>

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
    fun isBalanced(root: TreeNode?): Boolean {
        // Empty Tree is balanced
        if (root == null) {
            return true
        }
        // Get max height of subtree child
        // Get max height of subtree child
        // compare height difference (cannot be more than 1)
        var leftHeight = 0
        var rightHeight = 0
        if (root.left != null) {
            leftHeight = getMaxHeight(root.left)
        }
        if (root.right != null) {
            rightHeight = getMaxHeight(root.right)
        }
        val heightDiff = Math.abs(leftHeight - rightHeight)
        // if passes height check
        //  - Check if left subtree is balanced and if the right subtree is balanced
        //  - If one of both are imbalanced, then the tree is imbalanced
        return heightDiff <= 1 && isBalanced(root.left) && isBalanced(root.right)
    }

    private fun getMaxHeight(root: TreeNode?): Int {
        if (root == null) {
            return 0
        }
        var leftHeight = 0
        var rightHeight = 0
        // Left
        if (root.left != null) {
            leftHeight = getMaxHeight(root.left)
        }
        // Right
        if (root.right != null) {
            rightHeight = getMaxHeight(root.right)
        }
        return if (leftHeight > rightHeight) {
            1 + leftHeight
        } else {
            1 + rightHeight
        }
    }
}
```