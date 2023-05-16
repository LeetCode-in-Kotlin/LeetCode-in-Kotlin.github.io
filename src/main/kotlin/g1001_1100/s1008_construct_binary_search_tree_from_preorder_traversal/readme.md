[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1008\. Construct Binary Search Tree from Preorder Traversal

Medium

Given an array of integers preorder, which represents the **preorder traversal** of a BST (i.e., **binary search tree**), construct the tree and return _its root_.

It is **guaranteed** that there is always possible to find a binary search tree with the given requirements for the given test cases.

A **binary search tree** is a binary tree where for every node, any descendant of `Node.left` has a value **strictly less than** `Node.val`, and any descendant of `Node.right` has a value **strictly greater than** `Node.val`.

A **preorder traversal** of a binary tree displays the value of the node first, then traverses `Node.left`, then traverses `Node.right`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

**Input:** preorder = [8,5,1,7,10,12]

**Output:** [8,5,10,1,7,null,12]

**Example 2:**

**Input:** preorder = [1,3]

**Output:** [1,null,3]

**Constraints:**

*   `1 <= preorder.length <= 100`
*   `1 <= preorder[i] <= 1000`
*   All the values of `preorder` are **unique**.

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
    private var i = 0
    fun bstFromPreorder(preorder: IntArray): TreeNode? {
        return bstFromPreorder(preorder, Int.MAX_VALUE)
    }

    private fun bstFromPreorder(preorder: IntArray, bound: Int): TreeNode? {
        if (i == preorder.size || preorder[i] > bound) {
            return null
        }
        val root = TreeNode(preorder[i++])
        root.left = bstFromPreorder(preorder, root.`val`)
        root.right = bstFromPreorder(preorder, bound)
        return root
    }
}
```