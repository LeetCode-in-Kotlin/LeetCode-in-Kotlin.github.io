[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 543\. Diameter of Binary Tree

Easy

Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

**Input:** root = [1,2,3,4,5]

**Output:** 3

**Explanation:** 3 is the length of the path [4,2,1,3] or [5,2,1,3].

**Example 2:**

**Input:** root = [1,2]

**Output:** 1

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   `-100 <= Node.val <= 100`

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
    private var diameter = 0

    fun diameterOfBinaryTree(root: TreeNode?): Int {
        diameter = 0
        diameterOfBinaryTreeUtil(root)
        return diameter
    }

    private fun diameterOfBinaryTreeUtil(root: TreeNode?): Int {
        if (root == null) {
            return 0
        }
        val leftLength = if (root.left != null) 1 + diameterOfBinaryTreeUtil(root.left) else 0
        val rightLength = if (root.right != null) 1 + diameterOfBinaryTreeUtil(root.right) else 0
        diameter = Math.max(diameter, leftLength + rightLength)
        return Math.max(leftLength, rightLength)
    }
}
```