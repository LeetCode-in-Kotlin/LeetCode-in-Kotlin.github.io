[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 199\. Binary Tree Right Side View

Medium

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

**Input:** root = [1,2,3,null,5,null,4]

**Output:** [1,3,4]

**Example 2:**

**Input:** root = [1,null,3]

**Output:** [1,3]

**Example 3:**

**Input:** root = []

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 100]`.
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
    fun rightSideView(root: TreeNode?): List<Int> {
        val list: MutableList<Int> = ArrayList()
        recurse(root, 0, list)
        return list
    }

    private fun recurse(node: TreeNode?, level: Int, list: MutableList<Int>) {
        if (node != null) {
            if (list.size < level + 1) {
                list.add(node.`val`)
            }
            recurse(node.right, level + 1, list)
            recurse(node.left, level + 1, list)
        }
    }
}
```