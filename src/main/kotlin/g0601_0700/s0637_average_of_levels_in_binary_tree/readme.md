[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 637\. Average of Levels in Binary Tree

Easy

Given the `root` of a binary tree, return _the average value of the nodes on each level in the form of an array_. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/09/avg1-tree.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** [3.00000,14.50000,11.00000] Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/avg2-tree.jpg)

**Input:** root = [3,9,20,15,7]

**Output:** [3.00000,14.50000,11.00000]

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   <code>-2<sup>31</sup> <= Node.val <= 2<sup>31</sup> - 1</code>

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
    fun averageOfLevels(root: TreeNode?): List<Double> {
        val map: MutableMap<Int, Array<Double>> = HashMap()
        helper(root, map, 0)
        val result: MutableList<Double> = ArrayList()
        for (pair in map.values) {
            val avg = pair[1] / pair[0]
            result.add(avg)
        }
        return result
    }

    private fun helper(root: TreeNode?, map: MutableMap<Int, Array<Double>>, level: Int) {
        if (root == null) {
            return
        }
        val pair = if (map.containsKey(level)) map[level]!! else arrayOf(0.0, 0.0)
        pair[0] += 1.0
        pair[1] = pair[1] + root.`val`
        map[level] = pair
        helper(root.left, map, level + 1)
        helper(root.right, map, level + 1)
    }
}
```