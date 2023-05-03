[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 508\. Most Frequent Subtree Sum

Medium

Given the `root` of a binary tree, return the most frequent **subtree sum**. If there is a tie, return all the values with the highest frequency in any order.

The **subtree sum** of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/freq1-tree.jpg)

**Input:** root = [5,2,-3]

**Output:** [2,-3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/freq2-tree.jpg)

**Input:** root = [5,2,-5]

**Output:** [2]

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
    private val cache = mutableMapOf<Int, Int>()

    fun findFrequentTreeSum(root: TreeNode?): IntArray {
        treeSum(root)
        if (cache.isEmpty()) {
            return IntArray(0)
        }
        val max = cache.maxBy { it.value }.value
        return cache.filter { it.value == max }.map { it.key }.toIntArray()
    }

    private fun treeSum(node: TreeNode?): Int {
        return if (node == null) {
            0
        } else {
            val sum = node.`val` + treeSum(node.left) + treeSum(node.right)
            cache[sum] = cache.getOrDefault(sum, 0) + 1
            sum
        }
    }
}
```