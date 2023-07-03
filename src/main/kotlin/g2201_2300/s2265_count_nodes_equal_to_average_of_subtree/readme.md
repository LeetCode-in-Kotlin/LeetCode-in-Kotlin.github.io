[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2265\. Count Nodes Equal to Average of Subtree

Medium

Given the `root` of a binary tree, return _the number of nodes where the value of the node is equal to the **average** of the values in its **subtree**_.

**Note:**

*   The **average** of `n` elements is the **sum** of the `n` elements divided by `n` and **rounded down** to the nearest integer.
*   A **subtree** of `root` is a tree consisting of `root` and all of its descendants.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/15/image-20220315203925-1.png)

**Input:** root = [4,8,5,0,1,null,6]

**Output:** 5

**Explanation:**

For the node with value 4: The average of its subtree is (4 + 8 + 5 + 0 + 1 + 6) / 6 = 24 / 6 = 4.

For the node with value 5: The average of its subtree is (5 + 6) / 2 = 11 / 2 = 5.

For the node with value 0: The average of its subtree is 0 / 1 = 0.

For the node with value 1: The average of its subtree is 1 / 1 = 1.

For the node with value 6: The average of its subtree is 6 / 1 = 6.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/26/image-20220326133920-1.png)

**Input:** root = [1]

**Output:** 1

**Explanation:** For the node with value 1: The average of its subtree is 1 / 1 = 1.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `0 <= Node.val <= 1000`

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
    private var ans = 0
    fun averageOfSubtree(root: TreeNode?): Int {
        dfs(root)
        return ans
    }

    private fun dfs(node: TreeNode?): IntArray {
        if (node == null) {
            return intArrayOf(0, 0)
        }
        val left = dfs(node.left)
        val right = dfs(node.right)
        val currsum = left[0] + right[0] + node.`val`
        val currcount = left[1] + right[1] + 1
        if (currsum / currcount == node.`val`) {
            ++ans
        }
        return intArrayOf(currsum, currcount)
    }
}
```