[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 101\. Symmetric Tree

Easy

Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

**Input:** root = [1,2,2,3,4,4,3]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

**Input:** root = [1,2,2,null,3,null,3]

**Output:** false

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `-100 <= Node.val <= 100`

**Follow up:** Could you solve it both recursively and iteratively?

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
    fun isSymmetric(root: TreeNode?): Boolean {
        return if (root == null) {
            true
        } else {
            helper(root.left, root.right)
        }
    }

    private fun helper(leftNode: TreeNode?, rightNode: TreeNode?): Boolean {
        if (leftNode == null || rightNode == null) {
            return leftNode == null && rightNode == null
        }
        return if (leftNode.`val` != rightNode.`val`) {
            false
        } else {
            helper(leftNode.left, rightNode.right) && helper(leftNode.right, rightNode.left)
        }
    }
}
```