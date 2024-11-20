[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1080\. Insufficient Nodes in Root to Leaf Paths

Medium

Given the `root` of a binary tree and an integer `limit`, delete all **insufficient nodes** in the tree simultaneously, and return _the root of the resulting binary tree_.

A node is **insufficient** if every root to **leaf** path intersecting this node has a sum strictly less than `limit`.

A **leaf** is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/06/05/insufficient-11.png)

**Input:** root = [1,2,3,4,-99,-99,7,8,9,-99,-99,12,13,-99,14], limit = 1

**Output:** [1,2,3,4,null,null,7,8,9,null,14]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/06/05/insufficient-3.png)

**Input:** root = [5,4,8,11,null,17,4,7,1,null,null,5,3], limit = 22

**Output:** [5,4,8,11,null,17,4,7,null,null,null,5]

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/06/11/screen-shot-2019-06-11-at-83301-pm.png)

**Input:** root = [1,2,-3,-5,null,4,null], limit = -1

**Output:** [1,null,-3,4]

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 5000]`.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= limit <= 10<sup>9</sup></code>

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
    fun sufficientSubset(root: TreeNode?, limit: Int): TreeNode? {
        return if (sufficientSubset(root, limit, 0, root!!.left == null && root.right == null) < limit) null else root
    }

    fun sufficientSubset(root: TreeNode?, limit: Int, sum: Int, isLeaf: Boolean): Int {
        if (root != null) {
            val leftSum = sufficientSubset(
                root.left,
                limit,
                sum + root.`val`,
                root.left == null && root.right == null,
            )
            val rightSum = sufficientSubset(
                root.right,
                limit,
                sum + root.`val`,
                root.left == null && root.right == null,
            )
            if (leftSum < limit) {
                root.left = null
            }
            if (rightSum < limit) {
                root.right = null
            }
            return leftSum.coerceAtLeast(rightSum)
        }
        return if (isLeaf) sum else Int.MIN_VALUE
    }
}
```