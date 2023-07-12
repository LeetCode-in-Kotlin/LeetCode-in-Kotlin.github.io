[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 107\. Binary Tree Level Order Traversal II

Medium

Given the `root` of a binary tree, return _the bottom-up level order traversal of its nodes' values_. (i.e., from left to right, level by level from leaf to root).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** [[15,7],[9,20],[3]]

**Example 2:**

**Input:** root = [1]

**Output:** [[1]]

**Example 3:**

**Input:** root = []

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 2000]`.
*   `-1000 <= Node.val <= 1000`

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.Collections
import kotlin.collections.ArrayList

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
    private val order: MutableList<MutableList<Int>> = ArrayList()

    fun levelOrderBottom(root: TreeNode?): List<MutableList<Int>> {
        getOrder(root, 0)
        Collections.reverse(order)
        return order
    }

    fun getOrder(root: TreeNode?, level: Int) {
        if (root != null) {
            if (level + 1 > order.size) {
                order.add(ArrayList())
            }
            order[level].add(root.`val`)
            getOrder(root.left, level + 1)
            getOrder(root.right, level + 1)
        }
    }
}
```