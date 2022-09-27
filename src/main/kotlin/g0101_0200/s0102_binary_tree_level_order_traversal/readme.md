[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 102\. Binary Tree Level Order Traversal

Medium

Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** [[3],[9,20],[15,7]]

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
import java.util.ArrayList
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
    fun levelOrder(root: TreeNode?): List<List<Int>> {
        var localRoot: TreeNode? = root
        val result: MutableList<List<Int>> = ArrayList()
        if (localRoot == null) {
            return result
        }
        val queue: Queue<TreeNode> = LinkedList()
        queue.add(localRoot)
        queue.add(null)
        var level: MutableList<Int> = ArrayList()
        while (!queue.isEmpty()) {
            localRoot = queue.remove()
            while (!queue.isEmpty() && localRoot != null) {
                level.add(localRoot.`val`)
                if (localRoot.left != null) {
                    queue.add(localRoot.left)
                }
                if (localRoot.right != null) {
                    queue.add(localRoot.right)
                }
                localRoot = queue.remove()
            }
            result.add(level)
            level = ArrayList()
            if (!queue.isEmpty()) {
                queue.add(null)
            }
        }
        return result
    }
}
```