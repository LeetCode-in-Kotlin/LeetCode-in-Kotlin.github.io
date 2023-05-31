[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1028\. Recover a Tree From Preorder Traversal

Hard

We run a preorder depth-first search (DFS) on the `root` of a binary tree.

At each node in this traversal, we output `D` dashes (where `D` is the depth of this node), then we output the value of this node. If the depth of a node is `D`, the depth of its immediate child is `D + 1`. The depth of the `root` node is `0`.

If a node has only one child, that child is guaranteed to be **the left child**.

Given the output `traversal` of this traversal, recover the tree and return _its_ `root`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/08/recover-a-tree-from-preorder-traversal.png)

**Input:** traversal = "1-2--3--4-5--6--7"

**Output:** [1,2,5,3,4,6,7]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114101-pm.png)

**Input:** traversal = "1-2--3---4-5--6---7"

**Output:** [1,2,5,3,null,6,null,4,null,7]

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114955-pm.png)

**Input:** traversal = "1-401--349---90--88"

**Output:** [1,401,null,349,88,90]

**Constraints:**

*   The number of nodes in the original tree is in the range `[1, 1000]`.
*   <code>1 <= Node.val <= 10<sup>9</sup></code>

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
    private var ptr = 0

    fun recoverFromPreorder(traversal: String): TreeNode? {
        return find(traversal, 0)
    }

    private fun find(traversal: String, level: Int): TreeNode? {
        if (ptr == traversal.length) {
            return null
        }
        var i = ptr
        var count = 0
        while (traversal[i] == '-') {
            count++
            i++
        }
        return if (count == level) {
            val start = i
            while (i < traversal.length && traversal[i] != '-') {
                i++
            }
            val `val` = traversal.substring(start, i).toInt()
            ptr = i
            val root = TreeNode(`val`)
            root.left = find(traversal, level + 1)
            root.right = find(traversal, level + 1)
            root
        } else {
            null
        }
    }
}
```