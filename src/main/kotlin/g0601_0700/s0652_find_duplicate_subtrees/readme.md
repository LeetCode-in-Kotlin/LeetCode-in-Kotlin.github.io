[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 652\. Find Duplicate Subtrees

Medium

Given the `root` of a binary tree, return all **duplicate subtrees**.

For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are **duplicate** if they have the **same structure** with the **same node values**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

**Input:** root = [1,2,3,4,null,2,4,null,null,4]

**Output:** [[2,4],[4]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/16/e2.jpg)

**Input:** root = [2,1,1]

**Output:** [[1]]

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/08/16/e33.jpg)

**Input:** root = [2,2,2,3,null,3,null]

**Output:** [[2,3],[3]]

**Constraints:**

*   The number of the nodes in the tree will be in the range `[1, 5000]`
*   `-200 <= Node.val <= 200`

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
    fun findDuplicateSubtrees(root: TreeNode?): List<TreeNode> {
        val map: MutableMap<String, Int> = HashMap()
        val list: MutableList<TreeNode> = ArrayList<TreeNode>()
        helper(root, map, list)
        return list
    }

    private fun helper(root: TreeNode?, map: MutableMap<String, Int>, list: MutableList<TreeNode>): String {
        if (root == null) {
            return "#"
        }
        val key = helper(root.left, map, list) + "#" + helper(root.right, map, list) + "#" + root.`val`
        map[key] = map.getOrDefault(key, 0) + 1
        if (map[key] == 2) {
            list.add(root)
        }
        return key
    }
}
```