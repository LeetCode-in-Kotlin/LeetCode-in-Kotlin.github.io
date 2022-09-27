[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 95\. Unique Binary Search Trees II

Medium

Given an integer `n`, return _all the structurally unique **BST'**s (binary search trees), which has exactly_ `n` _nodes of unique values from_ `1` _to_ `n`. Return the answer in **any order**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

**Input:** n = 3

**Output:** [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]

**Example 2:**

**Input:** n = 1

**Output:** [[1]]

**Constraints:**

*   `1 <= n <= 8`

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
    fun generateTrees(n: Int): List<TreeNode?> {
        var result: MutableList<TreeNode?> = ArrayList<TreeNode?>()
        result.add(TreeNode(1))
        for (i in 2..n) {
            val nresult: MutableList<TreeNode?> = ArrayList<TreeNode?>()
            for (r in result) {
                var node = TreeNode(i, r, null)
                nresult.add(node)
                var previous: TreeNode? = r
                while (previous != null) {
                    node = TreeNode(i)
                    val cr: TreeNode? = copy(r)
                    insert(cr, node, previous)
                    previous = node.left
                    nresult.add(cr)
                }
            }
            result = nresult
        }
        return result
    }

    private fun insert(dest: TreeNode?, n: TreeNode, from: TreeNode) {
        if (dest != null && dest.`val` == from.`val`) {
            val h: TreeNode? = dest.right
            dest.right = n
            n.left = h
            return
        }
        if (dest != null) {
            insert(dest.right, n, from)
        }
    }

    private fun copy(n: TreeNode?): TreeNode? {
        return if (n == null) {
            null
        } else { TreeNode(n.`val`, copy(n.left), copy(n.right)) }
    }
}
```