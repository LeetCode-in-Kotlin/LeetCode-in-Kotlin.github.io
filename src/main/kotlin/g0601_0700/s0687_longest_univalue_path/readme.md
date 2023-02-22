[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 687\. Longest Univalue Path

Medium

Given the `root` of a binary tree, return _the length of the longest path, where each node in the path has the same value_. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

**Input:** root = [5,4,5,1,1,null,5]

**Output:** 2

**Explanation:** The shown image shows that the longest path of the same value (i.e. 5).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

**Input:** root = [1,4,5,4,4,null,5]

**Output:** 2

**Explanation:** The shown image shows that the longest path of the same value (i.e. 4).

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
*   `-1000 <= Node.val <= 1000`
*   The depth of the tree will not exceed `1000`.

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
    fun longestUnivaluePath(root: TreeNode?): Int {
        if (root == null) {
            return 0
        }
        val res = IntArray(1)
        preorderLongestSinglePathLen(root, res)
        return res[0]
    }

    private fun preorderLongestSinglePathLen(root: TreeNode?, res: IntArray): Int {
        if (root == null) {
            return -1
        }
        var left = preorderLongestSinglePathLen(root.left, res)
        var right = preorderLongestSinglePathLen(root.right, res)
        left = if (root.left == null || root.`val` == root.left!!.`val`) {
            left + 1
        } else {
            0
        }
        right = if (root.right == null || root.`val` == root.right!!.`val`) {
            right + 1
        } else {
            0
        }
        val longestPathLenPassingThroughRoot = left + right
        res[0] = res[0].coerceAtLeast(longestPathLenPassingThroughRoot)
        return left.coerceAtLeast(right)
    }
}
```