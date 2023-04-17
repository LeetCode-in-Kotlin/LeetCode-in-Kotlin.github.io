[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 894\. All Possible Full Binary Trees

Medium

Given an integer `n`, return _a list of all possible **full binary trees** with_ `n` _nodes_. Each node of each tree in the answer must have `Node.val == 0`.

Each element of the answer is the root node of one possible tree. You may return the final list of trees in **any order**.

A **full binary tree** is a binary tree where each node has exactly `0` or `2` children.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/22/fivetrees.png)

**Input:** n = 7

**Output:** [[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]

**Example 2:**

**Input:** n = 3

**Output:** [[0,0,0]]

**Constraints:**

*   `1 <= n <= 20`

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
    fun allPossibleFBT(n: Int): List<TreeNode> {
        if (n % 2 == 0) {
            // no complete binary tree possible
            return ArrayList()
        }
        val dp: Array<ArrayList<TreeNode>?> = arrayOfNulls(n + 1)
        // form left to right
        var i = 1
        while (i <= n) {
            helper(i, dp)
            i += 2
        }
        return dp[n]!!
    }

    // Using tabulation
    private fun helper(n: Int, dp: Array<ArrayList<TreeNode>?>) {
        if (n <= 0) {
            return
        }
        if (n == 1) {
            dp[1] = ArrayList()
            dp[1]!!.add(TreeNode(0))
            return
        }
        dp[n] = ArrayList()
        var i = 1
        while (i < n) {
            // left
            for (nodeL in dp[i]!!) {
                // right
                for (nodeR in dp[n - i - 1]!!) {
                    // 1 node used here
                    val root = TreeNode(0)
                    root.left = nodeL
                    root.right = nodeR
                    dp[n]!!.add(root)
                }
            }
            i += 2
        }
    }
}
```