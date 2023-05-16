[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 993\. Cousins in Binary Tree

Easy

Given the `root` of a binary tree with unique values and the values of two different nodes of the tree `x` and `y`, return `true` _if the nodes corresponding to the values_ `x` _and_ `y` _in the tree are **cousins**, or_ `false` _otherwise._

Two nodes of a binary tree are **cousins** if they have the same depth with different parents.

Note that in a binary tree, the root node is at the depth `0`, and children of each depth `k` node are at the depth `k + 1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/12/q1248-01.png)

**Input:** root = [1,2,3,4], x = 4, y = 3

**Output:** false

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/02/12/q1248-02.png)

**Input:** root = [1,2,3,null,4,null,5], x = 5, y = 4

**Output:** true

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/02/13/q1248-03.png)

**Input:** root = [1,2,3,null,4], x = 2, y = 3

**Output:** false

**Constraints:**

*   The number of nodes in the tree is in the range `[2, 100]`.
*   `1 <= Node.val <= 100`
*   Each node has a **unique** value.
*   `x != y`
*   `x` and `y` are exist in the tree.

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
    fun isCousins(root: TreeNode?, x: Int, y: Int): Boolean {
        return !isSiblings(root, x, y) && isSameLevels(root, x, y)
    }

    private fun isSameLevels(root: TreeNode?, x: Int, y: Int): Boolean {
        return findLevel(root, x, 0) == findLevel(root, y, 0)
    }

    private fun findLevel(root: TreeNode?, x: Int, level: Int): Int {
        if (root == null) {
            return -1
        }
        if (root.`val` == x) {
            return level
        }
        val leftLevel = findLevel(root.left, x, level + 1)
        return if (leftLevel == -1) {
            findLevel(root.right, x, level + 1)
        } else {
            leftLevel
        }
    }

    private fun isSiblings(root: TreeNode?, x: Int, y: Int): Boolean {
        if (root == null) {
            return false
        }
        // Check children first
        val leftSubTreeContainsCousins = isSiblings(root.left, x, y)
        val rightSubTreeContainsCousins = isSiblings(root.right, x, y)
        if (leftSubTreeContainsCousins || rightSubTreeContainsCousins) {
            return true
        }
        return if (root.left == null || root.right == null) {
            false
        } else root.left!!.`val` == x && root.right!!.`val` == y ||
            root.right!!.`val` == x && root.left!!.`val` == y
    }
}
```