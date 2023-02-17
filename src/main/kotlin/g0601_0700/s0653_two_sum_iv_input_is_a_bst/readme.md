[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 653\. Two Sum IV - Input is a BST

Easy

Given the `root` of a binary search tree and an integer `k`, return `true` _if there exist two elements in the BST such that their sum is equal to_ `k`, _or_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_1.jpg)

**Input:** root = [5,3,6,2,4,null,7], k = 9

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_2.jpg)

**Input:** root = [5,3,6,2,4,null,7], k = 28

**Output:** false

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-10<sup>4</sup> <= Node.val <= 10<sup>4</sup></code>
*   `root` is guaranteed to be a **valid** binary search tree.
*   <code>-10<sup>5</sup> <= k <= 10<sup>5</sup></code>

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
    fun findTarget(root: TreeNode?, k: Int): Boolean {
        if (root == null) {
            return false
        }
        val res: MutableList<Int> = ArrayList()
        inOrder(res, root)
        var i = 0
        var j = res.size - 1
        while (i < j) {
            val val1 = res[i]
            val val2 = res[j]
            if (val1 + val2 == k) {
                return true
            } else if (val1 + val2 < k) {
                i++
            } else {
                j--
            }
        }
        return false
    }

    private fun inOrder(res: MutableList<Int>, root: TreeNode?) {
        if (root == null) {
            return
        }
        inOrder(res, root.left)
        res.add(root.`val`)
        inOrder(res, root.right)
    }
}
```