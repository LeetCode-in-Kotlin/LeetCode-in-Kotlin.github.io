[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 105\. Construct Binary Tree from Preorder and Inorder Traversal

Medium

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]

**Output:** [3,9,20,null,null,15,7]

**Example 2:**

**Input:** preorder = [-1], inorder = [-1]

**Output:** [-1]

**Constraints:**

*   `1 <= preorder.length <= 3000`
*   `inorder.length == preorder.length`
*   `-3000 <= preorder[i], inorder[i] <= 3000`
*   `preorder` and `inorder` consist of **unique** values.
*   Each value of `inorder` also appears in `preorder`.
*   `preorder` is **guaranteed** to be the preorder traversal of the tree.
*   `inorder` is **guaranteed** to be the inorder traversal of the tree.

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.HashMap

class Solution {
    private var j = 0
    private val map: MutableMap<Int, Int> = HashMap()
    fun get(key: Int): Int {
        return map[key]!!
    }

    private fun answer(preorder: IntArray, inorder: IntArray, start: Int, end: Int): TreeNode? {
        if (start > end || j > preorder.size) {
            return null
        }
        val value = preorder[j++]
        val index = get(value)
        val node = TreeNode(value)
        node.left = answer(preorder, inorder, start, index - 1)
        node.right = answer(preorder, inorder, index + 1, end)
        return node
    }

    fun buildTree(preorder: IntArray, inorder: IntArray): TreeNode? {
        j = 0
        for (i in preorder.indices) {
            map[inorder[i]] = i
        }
        return answer(preorder, inorder, 0, preorder.size - 1)
    }
}
```