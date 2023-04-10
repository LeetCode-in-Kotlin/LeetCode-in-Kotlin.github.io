[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 889\. Construct Binary Tree from Preorder and Postorder Traversal

Medium

Given two integer arrays, `preorder` and `postorder` where `preorder` is the preorder traversal of a binary tree of **distinct** values and `postorder` is the postorder traversal of the same tree, reconstruct and return _the binary tree_.

If there exist multiple answers, you can **return any** of them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)

**Input:** preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]

**Output:** [1,2,3,4,5,6,7]

**Example 2:**

**Input:** preorder = [1], postorder = [1]

**Output:** [1]

**Constraints:**

*   `1 <= preorder.length <= 30`
*   `1 <= preorder[i] <= preorder.length`
*   All the values of `preorder` are **unique**.
*   `postorder.length == preorder.length`
*   `1 <= postorder[i] <= postorder.length`
*   All the values of `postorder` are **unique**.
*   It is guaranteed that `preorder` and `postorder` are the preorder traversal and postorder traversal of the same binary tree.

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
    fun constructFromPrePost(preorder: IntArray, postorder: IntArray): TreeNode? {
        return if (preorder.isEmpty() || preorder.size != postorder.size) {
            null
        } else buildTree(preorder, 0, preorder.size - 1, postorder, 0, postorder.size - 1)
    }

    private fun buildTree(
        preorder: IntArray,
        preStart: Int,
        preEnd: Int,
        postorder: IntArray,
        postStart: Int,
        postEnd: Int
    ): TreeNode? {
        if (preStart > preEnd || postStart > postEnd) {
            return null
        }
        val data = preorder[preStart]
        val root = TreeNode(data)
        if (preStart == preEnd) {
            return root
        }
        var offset = postStart
        while (offset <= preEnd) {
            if (postorder[offset] == preorder[preStart + 1]) {
                break
            }
            offset++
        }
        root.left = buildTree(
            preorder,
            preStart + 1,
            preStart + offset - postStart + 1,
            postorder,
            postStart,
            offset
        )
        root.right = buildTree(
            preorder,
            preStart + offset - postStart + 2,
            preEnd,
            postorder,
            offset + 1,
            postEnd - 1
        )
        return root
    }
}
```