[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 145\. Binary Tree Postorder Traversal

Easy

Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/pre1.jpg)

**Input:** root = [1,null,2,3]

**Output:** [3,2,1]

**Example 2:**

**Input:** root = []

**Output:** []

**Example 3:**

**Input:** root = [1]

**Output:** [1]

**Constraints:**

*   The number of the nodes in the tree is in the range `[0, 100]`.
*   `-100 <= Node.val <= 100`

**Follow up:** Recursive solution is trivial, could you do it iteratively?

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
    fun postorderTraversal(root: TreeNode?): MutableList<Int> {
        if (root == null) {
            return ArrayList()
        }
        val res = postorderTraversal(root.left)
        res.addAll(postorderTraversal(root.right))
        res.add(root.`val`)
        return res
    }
}
```