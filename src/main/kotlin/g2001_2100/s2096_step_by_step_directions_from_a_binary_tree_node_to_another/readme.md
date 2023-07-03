[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2096\. Step-By-Step Directions From a Binary Tree Node to Another

Medium

You are given the `root` of a **binary tree** with `n` nodes. Each node is uniquely assigned a value from `1` to `n`. You are also given an integer `startValue` representing the value of the start node `s`, and a different integer `destValue` representing the value of the destination node `t`.

Find the **shortest path** starting from node `s` and ending at node `t`. Generate step-by-step directions of such path as a string consisting of only the **uppercase** letters `'L'`, `'R'`, and `'U'`. Each letter indicates a specific direction:

*   `'L'` means to go from a node to its **left child** node.
*   `'R'` means to go from a node to its **right child** node.
*   `'U'` means to go from a node to its **parent** node.

Return _the step-by-step directions of the **shortest path** from node_ `s` _to node_ `t`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg1.png)

**Input:** root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6

**Output:** "UURL"

**Explanation:** The shortest path is: 3 → 1 → 5 → 2 → 6. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg2.png)

**Input:** root = [2,1], startValue = 2, destValue = 1

**Output:** "L"

**Explanation:** The shortest path is: 2 → 1. 

**Constraints:**

*   The number of nodes in the tree is `n`.
*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= Node.val <= n`
*   All the values in the tree are **unique**.
*   `1 <= startValue, destValue <= n`
*   `startValue != destValue`

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
    private fun find(n: TreeNode?, `val`: Int, sb: StringBuilder): Boolean {
        if (n!!.`val` == `val`) {
            return true
        }
        if (n.left != null && find(n.left, `val`, sb)) {
            sb.append("L")
        } else if (n.right != null && find(n.right, `val`, sb)) {
            sb.append("R")
        }
        return sb.isNotEmpty()
    }

    fun getDirections(root: TreeNode?, startValue: Int, destValue: Int): String {
        val s = StringBuilder()
        val d = StringBuilder()
        find(root, startValue, s)
        find(root, destValue, d)
        var i = 0
        val maxI = d.length.coerceAtMost(s.length)
        while (i < maxI && s[s.length - i - 1] == d[d.length - i - 1]) {
            ++i
        }
        val result = StringBuilder()
        for (j in 0 until s.length - i) {
            result.append("U")
        }
        result.append(d.reverse().substring(i))
        return result.toString()
    }
}
```