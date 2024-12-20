[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 113\. Path Sum II

Medium

Given the `root` of a binary tree and an integer `targetSum`, return _all **root-to-leaf** paths where the sum of the node values in the path equals_ `targetSum`_. Each path should be returned as a list of the node **values**, not node references_.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

**Input:** root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22

**Output:** [[5,4,11,2],[5,8,4,5]]

**Explanation:** There are two paths whose sum equals targetSum:

5 + 4 + 11 + 2 = 22 

5 + 8 + 4 + 5 = 22

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

**Input:** root = [1,2,3], targetSum = 5

**Output:** []

**Example 3:**

**Input:** root = [1,2], targetSum = 0

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 5000]`.
*   `-1000 <= Node.val <= 1000`
*   `-1000 <= targetSum <= 1000`

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
    fun pathSum(root: TreeNode?, targetSum: Int): List<List<Int>> {
        val res: MutableList<List<Int>> = ArrayList()
        if (root == null) {
            return res
        }
        recur(res, ArrayList(), 0, targetSum, root)
        return res
    }

    private fun recur(
        res: MutableList<List<Int>>,
        al: ArrayList<Int>,
        sum: Int,
        targetSum: Int,
        root: TreeNode?,
    ) {
        var sum = sum
        if (root == null) {
            return
        }
        al.add(root.`val`)
        sum += root.`val`
        if (sum == targetSum && root.left == null && root.right == null) {
            res.add(ArrayList(al))
        }
        recur(res, al, sum, targetSum, root.left)
        recur(res, al, sum, targetSum, root.right)
        al.removeAt(al.size - 1)
    }
}
```