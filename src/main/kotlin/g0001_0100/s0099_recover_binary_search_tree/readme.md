[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 99\. Recover Binary Search Tree

Medium

You are given the `root` of a binary search tree (BST), where the values of **exactly** two nodes of the tree were swapped by mistake. _Recover the tree without changing its structure_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover1.jpg)

**Input:** root = [1,3,null,null,2]

**Output:** [3,1,null,null,2]

**Explanation:** 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

**Input:** root = [3,1,4,null,null,2]

**Output:** [2,1,4,null,null,3]

**Explanation:** 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.

**Constraints:**

*   The number of nodes in the tree is in the range `[2, 1000]`.
*   <code>-2<sup>31</sup> <= Node.val <= 2<sup>31</sup> - 1</code>

**Follow up:** A solution using `O(n)` space is pretty straight-forward. Could you devise a constant `O(1)` space solution?

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
    private var prev: TreeNode? = null
    private var first: TreeNode? = null
    private var second: TreeNode? = null

    fun recoverTree(root: TreeNode?) {
        evalSwappedNodes(root)
        val temp: Int = first!!.`val`
        first!!.`val` = second!!.`val`
        second!!.`val` = temp
    }

    private fun evalSwappedNodes(curr: TreeNode?) {
        if (curr == null) {
            return
        }
        evalSwappedNodes(curr.left)
        if (prev != null && prev!!.`val` > curr.`val`) {
            if (first == null) {
                first = prev
            }
            second = curr
        }
        prev = curr
        evalSwappedNodes(curr.right)
    }
}
```