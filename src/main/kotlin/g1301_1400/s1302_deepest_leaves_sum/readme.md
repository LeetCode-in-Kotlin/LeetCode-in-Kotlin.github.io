[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1302\. Deepest Leaves Sum

Medium

Given the `root` of a binary tree, return _the sum of values of its deepest leaves_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

**Input:** root = [1,2,3,4,5,null,6,7,null,null,null,null,8]

**Output:** 15

**Example 2:**

**Input:** root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]

**Output:** 19

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.
*   `1 <= Node.val <= 100`

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.LinkedList
import java.util.Objects
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
    fun deepestLeavesSum(root: TreeNode?): Int {
        val queue: Queue<TreeNode?> = LinkedList()
        queue.offer(root)
        var sum = 0
        while (queue.isNotEmpty()) {
            val size = queue.size
            sum = 0
            for (i in 0 until size) {
                val curr = queue.poll()
                sum += Objects.requireNonNull(curr)!!.`val`
                if (curr!!.left != null) {
                    queue.offer(curr.left)
                }
                if (curr.right != null) {
                    queue.offer(curr.right)
                }
            }
        }
        return sum
    }
}
```