[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 897\. Increasing Order Search Tree

Easy

Given the `root` of a binary search tree, rearrange the tree in **in-order** so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

**Input:** root = [5,3,6,2,4,null,8,1,null,null,null,7,9]

**Output:** [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/17/ex2.jpg)

**Input:** root = [5,1,7]

**Output:** [1,null,5,null,7]

**Constraints:**

*   The number of nodes in the given tree will be in the range `[1, 100]`.
*   `0 <= Node.val <= 1000`

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
    fun increasingBST(root: TreeNode?): TreeNode {
        val list: MutableList<TreeNode> = LinkedList<TreeNode>()
        traverse(root, list)
        for (i in 1 until list.size) {
            list[i - 1].right = list[i]
            list[i].left = null
        }
        return list[0]
    }

    private fun traverse(root: TreeNode?, list: MutableList<TreeNode>) {
        if (root != null) {
            traverse(root.left, list)
            list.add(root)
            traverse(root.right, list)
        }
    }
}
```