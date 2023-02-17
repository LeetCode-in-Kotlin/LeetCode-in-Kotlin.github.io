[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 671\. Second Minimum Node In a Binary Tree

Easy

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly `two` or `zero` sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property `root.val = min(root.left.val, root.right.val)` always holds.

Given such a binary tree, you need to output the **second minimum** value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/15/smbt1.jpg)

**Input:** root = [2,2,5,null,null,5,7]

**Output:** 5

**Explanation:** The smallest value is 2, the second smallest value is 5.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/15/smbt2.jpg)

**Input:** root = [2,2,2]

**Output:** -1

**Explanation:** The smallest value is 2, but there isn't any second smallest value.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 25]`.
*   <code>1 <= Node.val <= 2<sup>31</sup> - 1</code>
*   `root.val == min(root.left.val, root.right.val)` for each internal node of the tree.

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
    var min = Int.MAX_VALUE
    var secMin = -1
    var diff = Int.MAX_VALUE
    fun findSecondMinimumValue(root: TreeNode?): Int {
        if (root == null) {
            return -1
        }
        if (root.`val` < min) {
            min = root.`val`
        }
        if (root.`val` != min && abs(root.`val` - min) < diff) {
            secMin = root.`val`
            diff = abs(root.`val` - min)
        }
        findSecondMinimumValue(root.left)
        findSecondMinimumValue(root.right)
        return secMin
    }
}
```