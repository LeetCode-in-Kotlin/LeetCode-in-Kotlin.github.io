[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1161\. Maximum Level Sum of a Binary Tree

Medium

Given the `root` of a binary tree, the level of its root is `1`, the level of its children is `2`, and so on.

Return the **smallest** level `x` such that the sum of all the values of nodes at level `x` is **maximal**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/03/capture.JPG)

**Input:** root = [1,7,0,7,-8,null,null]

**Output:** 2

**Explanation:**  

Level 1 sum = 1. 

Level 2 sum = 7 + 0 = 7. 

Level 3 sum = 7 + -8 = -1. 

So we return the level with the maximum sum which is level 2.

**Example 2:**

**Input:** root = [989,null,10250,98693,-89388,null,null,null,-32127]

**Output:** 2

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
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
    private var sums: MutableList<Int> = ArrayList()

    fun maxLevelSum(root: TreeNode?): Int {
        sums = ArrayList()
        find(root, 1)
        var ans = 1
        var maxv = Int.MIN_VALUE
        for (i in sums.indices) {
            if (sums[i] > maxv) {
                maxv = sums[i]
                ans = i + 1
            }
        }
        return ans
    }

    private fun find(root: TreeNode?, height: Int) {
        if (root == null) {
            return
        }
        if (sums.size < height) {
            sums.add(root.`val`)
        } else {
            sums[height - 1] = sums[height - 1] + root.`val`
        }
        find(root.left, height + 1)
        find(root.right, height + 1)
    }
}
```