[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 429\. N-ary Tree Level Order Traversal

Medium

Given an n-ary tree, return the _level order_ traversal of its nodes' values.

_Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples)._

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

**Input:** root = [1,null,3,2,4,null,5,6]

**Output:** [[1],[3,2,4],[5,6]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

**Input:** root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]

**Output:** [[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]

**Constraints:**

*   The height of the n-ary tree is less than or equal to `1000`
*   The total number of nodes is between <code>[0, 10<sup>4</sup>]</code>

## Solution

```kotlin
import com_github_leetcode.Node
import java.util.LinkedList
import java.util.Queue

/*
 * Definition for a Node.
 * class Node(var `val`: Int) {
 *     var neighbors: List<Node?> = listOf()
 * }
 */
class Solution {
    fun levelOrder(root: Node?): List<List<Int>> {
        val result: MutableList<List<Int>> = ArrayList()
        if (root == null) {
            return result
        }
        val queue: Queue<Node> = LinkedList<Node>()
        queue.offer(root)
        while (queue.isNotEmpty()) {
            val size: Int = queue.size
            val level: MutableList<Int> = ArrayList()
            for (i in 0 until size) {
                val currentNode: Node = queue.poll()
                level.add(currentNode.`val`)
                for (child in currentNode.neighbors) {
                    queue.offer(child)
                }
            }
            result.add(level)
        }
        return result
    }
}
```