[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3997\. Count Dominant Nodes in a Binary Tree

Medium

You are given the `root` of a complete binary tree.

A node `x` is called **dominant** if its value is equal to the maximum value among all nodes in the subtree rooted at `x`.

Return the number of dominant nodes in the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2026/06/13/tnew.png)

**Input:** root = [5,3,8,2,4,7,1]

**Output:** 5

**Explanation:**

*   The leaf nodes with values 2, 4, 7, and 1 are dominant.
*   The node with value 8 is dominant because its value is the maximum value in its subtree `[8, 7, 1]`.
*   Thus, the answer is 5.

**Example 2:**

![](https://assets.leetcode.com/uploads/2026/06/15/t9.png)

**Input:** root = [1,2,3,1,2]

**Output:** 4

**Explanation:**

*   The leaf nodes with values 1, 2, and 3 are dominant.
*   The node with value 2 whose subtree is `[2, 1, 2]` is dominant because its value is the maximum value in its subtree.
*   Thus, the answer is 4.

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>5</sup>]</code>.
*   <code>1 <= Node.val <= 10<sup>9</sup></code>
*   The tree is guaranteed to be a complete binary tree.

## Solution

```kotlin
import com_github_leetcode.TreeNode
import kotlin.math.max

/*
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private var dominantNode = 0

    fun countDominantNodes(root: TreeNode?): Int {
        dominantNode = 0
        dfs(root)
        return dominantNode
    }

    private fun dfs(root: TreeNode?): Int {
        if (root == null) {
            return Int.MIN_VALUE
        }
        if (root.left == null && root.right == null) {
            dominantNode++
            return root.`val`
        }
        val left = dfs(root.left)
        val right = dfs(root.right)
        val max = max(left, right)
        if (root.`val` >= max) {
            dominantNode++
        }
        return max(max, root.`val`)
    }
}
```