[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1325\. Delete Leaves With a Given Value

Medium

Given a binary tree `root` and an integer `target`, delete all the **leaf nodes** with value `target`.

Note that once you delete a leaf node with value `target`**,** if its parent node becomes a leaf node and has the value `target`, it should also be deleted (you need to continue doing that until you cannot).

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/01/09/sample_1_1684.png)**

**Input:** root = [1,2,3,2,null,2,4], target = 2

**Output:** [1,null,3,null,4]

**Explanation:** Leaf nodes in green with value (target = 2) are removed (Picture in left). After removing, new nodes become leaf nodes with value (target = 2) (Picture in center).

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/01/09/sample_2_1684.png)**

**Input:** root = [1,3,3,3,2], target = 3

**Output:** [1,3,null,null,2]

**Example 3:**

**![](https://assets.leetcode.com/uploads/2020/01/15/sample_3_1684.png)**

**Input:** root = [1,2,null,2,null,2], target = 2

**Output:** [1]

**Explanation:** Leaf nodes in green with value (target = 2) are removed at each step.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 3000]`.
*   `1 <= Node.val, target <= 1000`

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
    fun removeLeafNodes(root: TreeNode?, target: Int): TreeNode? {
        var root = root
        while (hasTargetLeafNodes(root, target)) {
            root = removeLeafNodes(target, root)
        }
        return root
    }

    private fun removeLeafNodes(target: Int, root: TreeNode?): TreeNode? {
        var root = root
        if (root == null) {
            return root
        }
        if (root.`val` == target && root.left == null && root.right == null) {
            root = null
            return root
        }
        if (root.left != null && root.left!!.`val` == target && root.left!!.left == null && root.left!!.right == null) {
            root.left = null
        }
        if (root.right != null && root.right!!.`val` == target && root.right!!.left == null &&
            root.right!!.right == null
        ) {
            root.right = null
        }
        removeLeafNodes(target, root.left)
        removeLeafNodes(target, root.right)
        return root
    }

    private fun hasTargetLeafNodes(root: TreeNode?, target: Int): Boolean {
        if (root == null) {
            return false
        }
        return if (root.left == null && root.right == null && root.`val` == target) {
            true
        } else hasTargetLeafNodes(root.left, target) || hasTargetLeafNodes(root.right, target)
    }
}
```