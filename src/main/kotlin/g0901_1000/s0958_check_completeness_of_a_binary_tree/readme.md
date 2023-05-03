[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 958\. Check Completeness of a Binary Tree

Medium

Given the `root` of a binary tree, determine if it is a _complete binary tree_.

In a **[complete binary tree](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between `1` and <code>2<sup>h</sup></code> nodes inclusive at the last level `h`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-1.png)

**Input:** root = [1,2,3,4,5,6]

**Output:** true

**Explanation:** Every level before the last is full (ie. levels with node-values {1} and {2, 3}), and all nodes in the last level ({4, 5, 6}) are as far left as possible.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-2.png)

**Input:** root = [1,2,3,4,5,null,7]

**Output:** false

**Explanation:** The node with value 7 isn't as far left as possible.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 100]`.
*   `1 <= Node.val <= 1000`

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
    fun isCompleteTree(root: TreeNode?): Boolean {
        if (root == null) {
            return true
        }
        val queue: LinkedList<TreeNode?> = LinkedList<TreeNode?>()
        queue.add(root.left)
        queue.add(root.right)
        var seenNull = false
        while (!queue.isEmpty()) {
            val node: TreeNode? = queue.poll()
            if (node == null) {
                seenNull = true
            } else {
                if (seenNull) {
                    return false
                }
                queue.add(node.left)
                queue.add(node.right)
            }
        }
        return true
    }
}
```