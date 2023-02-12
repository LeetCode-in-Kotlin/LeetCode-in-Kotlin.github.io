[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 623\. Add One Row to Tree

Medium

Given the `root` of a binary tree and two integers `val` and `depth`, add a row of nodes with value `val` at the given depth `depth`.

Note that the `root` node is at depth `1`.

The adding rule is:

*   Given the integer `depth`, for each not null tree node `cur` at the depth `depth - 1`, create two tree nodes with value `val` as `cur`'s left subtree root and right subtree root.
*   `cur`'s original left subtree should be the left subtree of the new left subtree root.
*   `cur`'s original right subtree should be the right subtree of the new right subtree root.
*   If `depth == 1` that means there is no depth `depth - 1` at all, then create a tree node with value `val` as the new root of the whole original tree, and the original tree is the new root's left subtree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/15/addrow-tree.jpg)

**Input:** root = [4,2,6,3,1,5], val = 1, depth = 2

**Output:** [4,1,1,2,null,null,6,3,1,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/11/add2-tree.jpg)

**Input:** root = [4,2,null,3,1], val = 1, depth = 3

**Output:** [4,2,null,1,1,3,null,null,1]

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   The depth of the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   `-100 <= Node.val <= 100`
*   <code>-10<sup>5</sup> <= val <= 10<sup>5</sup></code>
*   `1 <= depth <= the depth of tree + 1`

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
    fun addOneRow(root: TreeNode?, `val`: Int, depth: Int): TreeNode? {
        if (depth == 1) {
            val newRoot = TreeNode(`val`)
            newRoot.left = root
            return newRoot
        }
        dfs(root!!, depth - 2, `val`)
        return root
    }

    private fun dfs(node: TreeNode, depth: Int, `val`: Int) {
        if (depth == 0) {
            val left = TreeNode(`val`)
            val right = TreeNode(`val`)
            left.left = node.left
            right.right = node.right
            node.left = left
            node.right = right
        } else {
            if (node.left != null) {
                dfs(node.left!!, depth - 1, `val`)
            }
            if (node.right != null) {
                dfs(node.right!!, depth - 1, `val`)
            }
        }
    }
}
```