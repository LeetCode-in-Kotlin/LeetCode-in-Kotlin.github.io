[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 783\. Minimum Distance Between BST Nodes

Easy

Given the `root` of a Binary Search Tree (BST), return _the minimum difference between the values of any two different nodes in the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

**Input:** root = [4,2,6,1,3]

**Output:** 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

**Input:** root = [1,0,48,null,null,12,49]

**Output:** 1

**Constraints:**

*   The number of nodes in the tree is in the range `[2, 100]`.
*   <code>0 <= Node.val <= 10<sup>5</sup></code>

**Note:** This question is the same as 530: [https://leetcode.com/problems/minimum-absolute-difference-in-bst/](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

## Solution

```kotlin
import com_github_leetcode.TreeNode
import kotlin.math.abs

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
    private var prev = -1
    private var min = Int.MAX_VALUE
    fun minDiffInBST(root: TreeNode?): Int {
        if (root == null) {
            return min
        }
        minDiffInBST(root.left)
        if (prev != -1) {
            min = min.coerceAtMost(abs(root.`val` - prev))
        }
        prev = root.`val`
        minDiffInBST(root.right)
        return min
    }
}
```