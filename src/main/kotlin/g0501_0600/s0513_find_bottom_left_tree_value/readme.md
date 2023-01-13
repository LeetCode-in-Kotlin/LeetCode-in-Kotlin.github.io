[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 513\. Find Bottom Left Tree Value

Medium

Given the `root` of a binary tree, return the leftmost value in the last row of the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/14/tree1.jpg)

**Input:** root = [2,1,3]

**Output:** 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/14/tree2.jpg)

**Input:** root = [1,2,3,4,null,5,6,null,null,7]

**Output:** 7

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-2<sup>31</sup> <= Node.val <= 2<sup>31</sup> - 1</code>

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
    private fun func(root: TreeNode?, level: Int): IntArray? {
        if (root!!.left == null && root.right == null) {
            val a = IntArray(2)
            a[0] = root.`val`
            a[1] = level
            return a
        }
        var a: IntArray? = null
        var b: IntArray? = null
        if (root.left != null) {
            a = func(root.left, level + 1)
        }
        if (root.right != null) {
            b = func(root.right, level + 1)
        }
        return if (a == null) {
            b
        } else if (b == null) {
            a
        } else {
            if (a[1] >= b[1]) {
                a
            } else {
                b
            }
        }
    }

    fun findBottomLeftValue(root: TreeNode?): Int {
        val a = func(root, 0)
        return if (a != null && a.size > 0) {
            a[0]
        } else -1
    }
}
```