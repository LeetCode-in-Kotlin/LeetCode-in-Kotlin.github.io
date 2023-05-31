[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 111\. Minimum Depth of Binary Tree

Easy

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** 2

**Example 2:**

**Input:** root = [2,null,3,null,4,null,5,null,6]

**Output:** 5

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>5</sup>]</code>.
*   `-1000 <= Node.val <= 1000`

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
    fun minDepth(root: TreeNode?): Int {
        if (root == null) {
            return 0
        }
        val queue: Queue<TreeNode> = LinkedList()
        queue.add(root)
        var d = 0
        while (queue.isNotEmpty()) {
            val size: Int = queue.size
            for (i in 0 until size) {
                val current: TreeNode = queue.poll()
                if (current.left == null && current.right == null && d > 0) {
                    return d + 1
                }
                if (current.right != null) {
                    queue.add(current.right)
                }
                if (current.left != null) {
                    queue.add(current.left)
                }
            }
            d++
        }
        return d
    }
}
```