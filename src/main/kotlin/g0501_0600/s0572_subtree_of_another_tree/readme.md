[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 572\. Subtree of Another Tree

Easy

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)

**Input:** root = [3,4,5,1,2], subRoot = [4,1,2]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg)

**Input:** root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]

**Output:** false

**Constraints:**

*   The number of nodes in the `root` tree is in the range `[1, 2000]`.
*   The number of nodes in the `subRoot` tree is in the range `[1, 1000]`.
*   <code>-10<sup>4</sup> <= root.val <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= subRoot.val <= 10<sup>4</sup></code>

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
    private fun isSubtreeFound(root: TreeNode?, subRoot: TreeNode?): Boolean {
        if (root == null && subRoot == null) {
            return true
        }
        if (root == null || subRoot == null) {
            return false
        }
        return if (root.`val` == subRoot.`val`) {
            isSubtreeFound(root.left, subRoot.left) && isSubtree(root.right, subRoot.right)
        } else {
            false
        }
    }

    fun isSubtree(root: TreeNode?, subRoot: TreeNode?): Boolean {
        if (root == null && subRoot == null) {
            return true
        }
        return if (root == null || subRoot == null) {
            false
        } else isSubtreeFound(root, subRoot) ||
            isSubtree(root.left, subRoot) ||
            isSubtree(root.right, subRoot)
    }
}
```