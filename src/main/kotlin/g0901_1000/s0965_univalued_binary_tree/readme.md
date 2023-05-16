[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 965\. Univalued Binary Tree

Easy

A binary tree is **uni-valued** if every node in the tree has the same value.

Given the `root` of a binary tree, return `true` _if the given tree is **uni-valued**, or_ `false` _otherwise._

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_1.png)

**Input:** root = [1,1,1,1,1,null,1]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_2.png)

**Input:** root = [2,2,2,5,2]

**Output:** false

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 100]`.
*   `0 <= Node.val < 100`

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.LinkedList

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
    fun isUnivalTree(root: TreeNode?): Boolean {
        val `val`: Int = root!!.`val`
        val queue: LinkedList<TreeNode?> = LinkedList<TreeNode?>()
        queue.add(root)
        while (queue.isNotEmpty()) {
            val node: TreeNode? = queue.poll()
            if (node!!.`val` != `val`) {
                return false
            }
            if (node.left != null) {
                queue.add(node.left)
            }
            if (node.right != null) {
                queue.add(node.right)
            }
        }
        return true
    }
}
```