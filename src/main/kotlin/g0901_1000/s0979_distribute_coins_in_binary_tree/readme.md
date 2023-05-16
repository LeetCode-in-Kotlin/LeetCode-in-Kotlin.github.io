[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 979\. Distribute Coins in Binary Tree

Medium

You are given the `root` of a binary tree with `n` nodes where each `node` in the tree has `node.val` coins. There are `n` coins in total throughout the whole tree.

In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent.

Return _the **minimum** number of moves required to make every node have **exactly** one coin_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree1.png)

**Input:** root = [3,0,0]

**Output:** 2

**Explanation:** From the root of the tree, we move one coin to its left child, and one coin to its right child.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree2.png)

**Input:** root = [0,3,0]

**Output:** 3

**Explanation:** From the left child of the root, we move two coins to the root [taking two moves]. Then, we move one coin from the root of the tree to the right child.

**Constraints:**

*   The number of nodes in the tree is `n`.
*   `1 <= n <= 100`
*   `0 <= Node.val <= n`
*   The sum of all `Node.val` is `n`.

## Solution

```kotlin
import com_github_leetcode.TreeNode
import kotlin.math.abs

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
    private var num = 0
    fun distributeCoins(root: TreeNode?): Int {
        helper(root)
        return num
    }

    private fun helper(node: TreeNode?): Int {
        if (node == null) {
            return 0
        }
        val total: Int = node.`val` + helper(node.left) + helper(node.right)
        val leftover = total - 1
        num += abs(leftover)
        return leftover
    }
}
```