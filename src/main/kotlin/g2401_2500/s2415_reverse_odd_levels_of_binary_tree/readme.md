[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2415\. Reverse Odd Levels of Binary Tree

Medium

Given the `root` of a **perfect** binary tree, reverse the node values at each **odd** level of the tree.

*   For example, suppose the node values at level 3 are `[2,1,3,4,7,11,29,18]`, then it should become `[18,29,11,7,4,3,1,2]`.

Return _the root of the reversed tree_.

A binary tree is **perfect** if all parent nodes have two children and all leaves are on the same level.

The **level** of a node is the number of edges along the path between it and the root node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/07/28/first_case1.png)

**Input:** root = [2,3,5,8,13,21,34]

**Output:** [2,5,3,8,13,21,34]

**Explanation:**

The tree has only one odd level.

The nodes at level 1 are 3, 5 respectively, which are reversed and become 5, 3. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/07/28/second_case3.png)

**Input:** root = [7,13,11]

**Output:** [7,11,13]

**Explanation:**

The nodes at level 1 are 13, 11, which are reversed and become 11, 13. 

**Example 3:**

**Input:** root = [0,1,2,0,0,0,0,1,1,1,1,2,2,2,2]

**Output:** [0,2,1,0,0,0,0,2,2,2,2,1,1,1,1]

**Explanation:**

The odd levels have non-zero values.

The nodes at level 1 were 1, 2, and are 2, 1 after the reversal.

The nodes at level 3 were 1, 1, 1, 1, 2, 2, 2, 2, and are 2, 2, 2, 2, 1, 1, 1, 1 after the reversal. 

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 2<sup>14</sup>]</code>.
*   <code>0 <= Node.val <= 10<sup>5</sup></code>
*   `root` is a **perfect** binary tree.

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
    private val list: MutableList<Int> = ArrayList()
    fun reverseOddLevels(root: TreeNode): TreeNode? {
        solve(root)
        return enrich(list, 0)
    }

    private fun enrich(list: List<Int>, i: Int): TreeNode? {
        var root: TreeNode? = null
        if (i < list.size) {
            root = TreeNode(list[i])
            root.left = enrich(list, 2 * i + 1)
            root.right = enrich(list, 2 * i + 2)
        }
        return root
    }

    private fun solve(root: TreeNode) {
        val q: Queue<TreeNode?> = LinkedList()
        q.add(root)
        var level = 0
        while (q.isNotEmpty()) {
            val size = q.size
            val res: MutableList<Int> = ArrayList()
            for (i in 0 until size) {
                val cur = q.remove()
                res.add(cur!!.`val`)
                if (cur.left != null) {
                    q.add(cur.left)
                }
                if (cur.right != null) {
                    q.add(cur.right)
                }
            }
            if (level % 2 != 0) {
                for (i in res.indices.reversed()) {
                    list.add(res[i])
                }
            } else {
                list.addAll(res)
            }
            level++
        }
    }
}
```