[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1305\. All Elements in Two Binary Search Trees

Medium

Given two binary search trees `root1` and `root2`, return _a list containing all the integers from both trees sorted in **ascending** order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/18/q2-e1.png)

**Input:** root1 = [2,1,4], root2 = [1,0,3]

**Output:** [0,1,1,2,3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/18/q2-e5-.png)

**Input:** root1 = [1,null,8], root2 = [8,1]

**Output:** [1,1,8,8]

**Constraints:**

*   The number of nodes in each tree is in the range `[0, 5000]`.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>

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
    fun getAllElements(root1: TreeNode?, root2: TreeNode?): List<Int> {
        val list1 = getAllNodes(root1)
        val list2 = getAllNodes(root2)
        val merged: MutableList<Int> = ArrayList()
        merged.addAll(list1)
        merged.addAll(list2)
        merged.sort()
        return merged
    }

    private fun getAllNodes(root: TreeNode?): List<Int> {
        val list: MutableList<Int> = ArrayList()
        return inorder(root, list)
    }

    private fun inorder(root: TreeNode?, result: MutableList<Int>): List<Int> {
        if (root == null) {
            return result
        }
        inorder(root.left, result)
        result.add(root.`val`)
        return inorder(root.right, result)
    }
}
```