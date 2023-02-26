[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 700\. Search in a Binary Search Tree

Easy

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

**Input:** root = [4,2,7,1,3], val = 2

**Output:** [2,1,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/12/tree2.jpg)

**Input:** root = [4,2,7,1,3], val = 5

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 5000]`.
*   <code>1 <= Node.val <= 10<sup>7</sup></code>
*   `root` is a binary search tree.
*   <code>1 <= val <= 10<sup>7</sup></code>

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
@Suppress("NAME_SHADOWING")
class Solution {
    fun searchBST(root: TreeNode?, `val`: Int): TreeNode? {
        var root: TreeNode? = root
        while (root != null && root.`val` != `val`) {
            root = if (root.`val` > `val`) {
                root.left
            } else {
                root.right
            }
        }
        return root
    }
}
```