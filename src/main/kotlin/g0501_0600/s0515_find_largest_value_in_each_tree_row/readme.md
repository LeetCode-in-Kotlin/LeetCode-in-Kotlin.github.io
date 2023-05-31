[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 515\. Find Largest Value in Each Tree Row

Medium

Given the `root` of a binary tree, return _an array of the largest value in each row_ of the tree **(0-indexed)**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg)

**Input:** root = [1,3,2,5,3,null,9]

**Output:** [1,3,9]

**Example 2:**

**Input:** root = [1,2,3]

**Output:** [1,3]

**Constraints:**

*   The number of nodes in the tree will be in the range <code>[0, 10<sup>4</sup>]</code>.
*   <code>-2<sup>31</sup> <= Node.val <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.LinkedList
import java.util.Queue

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
    fun largestValues(root: TreeNode?): List<Int> {
        val list: MutableList<Int> = ArrayList()
        val queue: Queue<TreeNode?> = LinkedList()
        if (root != null) {
            queue.offer(root)
            while (queue.isNotEmpty()) {
                var max = Int.MIN_VALUE
                val size = queue.size
                for (i in 0 until size) {
                    val curr = queue.poll()
                    max = Math.max(max, curr!!.`val`)
                    if (curr.left != null) {
                        queue.offer(curr.left)
                    }
                    if (curr.right != null) {
                        queue.offer(curr.right)
                    }
                }
                list.add(max)
            }
        }
        return list
    }
}
```