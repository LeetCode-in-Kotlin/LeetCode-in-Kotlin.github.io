[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 559\. Maximum Depth of N-ary Tree

Easy

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

_Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples)._

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

**Input:** root = [1,null,3,2,4,null,5,6]

**Output:** 3

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

**Input:** root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]

**Output:** 5

**Constraints:**

*   The total number of nodes is in the range <code>[0, 10<sup>4</sup>]</code>.
*   The depth of the n-ary tree is less than or equal to `1000`.

## Solution

```kotlin
import com_github_leetcode.Node

/*
 * Definition for a Node.
 * class Node(var `val`: Int) {
 *     var neighbors: List<Node?> = listOf()
 * }
 */
class Solution {
    private var max = 0

    fun maxDepth(root: Node?): Int {
        if (root == null) {
            return 0
        }
        if (root.neighbors.isEmpty()) {
            return 1
        }
        for (child in root.neighbors) {
            findDepth(child, 1)
        }
        return max
    }

    private fun findDepth(n: Node?, d: Int) {
        if (n!!.neighbors.isNotEmpty()) {
            for (no in n.neighbors) {
                findDepth(no, d + 1)
            }
        } else {
            max = Math.max(max, d + 1)
        }
    }
}
```