[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1022\. Sum of Root To Leaf Binary Numbers

Easy

You are given the `root` of a binary tree where each node has a value `0` or `1`. Each root-to-leaf path represents a binary number starting with the most significant bit.

*   For example, if the path is `0 -> 1 -> 1 -> 0 -> 1`, then this could represent `01101` in binary, which is `13`.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf. Return _the sum of these numbers_.

The test cases are generated so that the answer fits in a **32-bits** integer.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)

**Input:** root = [1,0,1,0,1,0,1]

**Output:** 22

**Explanation:** (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22

**Example 2:**

**Input:** root = [0]

**Output:** 0

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `Node.val` is `0` or `1`.

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
    fun sumRootToLeaf(root: TreeNode?): Int {
        val paths: MutableList<List<Int>> = ArrayList()
        dfs(root, paths, ArrayList())
        var sum = 0
        for (list in paths) {
            var num = 0
            for (i in list) {
                num = (num shl 1) + i
            }
            sum += num
        }
        return sum
    }

    private fun dfs(root: TreeNode?, paths: MutableList<List<Int>>, path: MutableList<Int>) {
        path.add(root!!.`val`)
        if (root.left != null) {
            dfs(root.left!!, paths, path)
            path.removeAt(path.size - 1)
        }
        if (root.right != null) {
            dfs(root.right!!, paths, path)
            path.removeAt(path.size - 1)
        }
        if (root.left == null && root.right == null) {
            paths.add(ArrayList(path))
        }
    }
}
```