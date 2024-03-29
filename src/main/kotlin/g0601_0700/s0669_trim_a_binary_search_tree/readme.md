[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 669\. Trim a Binary Search Tree

Medium

Given the `root` of a binary search tree and the lowest and highest boundaries as `low` and `high`, trim the tree so that all its elements lies in `[low, high]`. Trimming the tree should **not** change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a **unique answer**.

Return _the root of the trimmed binary search tree_. Note that the root may change depending on the given bounds.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/09/trim1.jpg)

**Input:** root = [1,0,2], low = 1, high = 2

**Output:** [1,null,2]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/09/trim2.jpg)

**Input:** root = [3,0,4,null,2,null,null,1], low = 1, high = 3

**Output:** [3,2,null,1]

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 10<sup>4</sup></code>
*   The value of each node in the tree is **unique**.
*   `root` is guaranteed to be a valid binary search tree.
*   <code>0 <= low <= high <= 10<sup>4</sup></code>

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
    fun trimBST(root: TreeNode?, l: Int, r: Int): TreeNode? {
        if (root == null) {
            return root
        }
        if (root.`val` > r) {
            return trimBST(root.left, l, r)
        }
        if (root.`val` < l) {
            return trimBST(root.right, l, r)
        }
        root.left = trimBST(root.left, l, r)
        root.right = trimBST(root.right, l, r)
        return root
    }
}
```