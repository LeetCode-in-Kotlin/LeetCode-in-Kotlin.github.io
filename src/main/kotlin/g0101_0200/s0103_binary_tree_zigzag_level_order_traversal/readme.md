[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 103\. Binary Tree Zigzag Level Order Traversal

Medium

Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** [[3],[20,9],[15,7]]

**Example 2:**

**Input:** root = [1]

**Output:** [[1]]

**Example 3:**

**Input:** root = []

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 2000]`.
*   `-100 <= Node.val <= 100`

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.LinkedList
import java.util.Queue

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
    fun zigzagLevelOrder(root: TreeNode?): List<List<Int>> {
        val result: MutableList<List<Int>> = ArrayList()
        if (root == null) {
            return result
        }
        val q: Queue<TreeNode> = LinkedList()
        q.add(root)
        q.add(null)
        var zig = true
        var level = LinkedList<Int>()
        while (q.isNotEmpty()) {
            var node: TreeNode? = q.remove()
            while (q.isNotEmpty() && node != null) {
                if (zig) {
                    level.add(node.`val`)
                } else {
                    level.addFirst(node.`val`)
                }
                if (node.left != null) {
                    q.add(node.left)
                }
                if (node.right != null) {
                    q.add(node.right)
                }
                node = q.remove()
            }
            result.add(level)
            zig = !zig
            level = LinkedList()
            if (q.isNotEmpty()) {
                q.add(null)
            }
        }
        return result
    }
}
```