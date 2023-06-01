[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1104\. Path In Zigzag Labelled Binary Tree

Medium

In an infinite binary tree where every node has two children, the nodes are labelled in row order.

In the odd numbered rows (ie., the first, third, fifth,...), the labelling is left to right, while in the even numbered rows (second, fourth, sixth,...), the labelling is right to left.

![](https://assets.leetcode.com/uploads/2019/06/24/tree.png)

Given the `label` of a node in this tree, return the labels in the path from the root of the tree to the node with that `label`.

**Example 1:**

**Input:** label = 14

**Output:** [1,3,4,14]

**Example 2:**

**Input:** label = 26

**Output:** [1,2,6,10,26]

**Constraints:**

*   `1 <= label <= 10^6`

## Solution

```kotlin
import java.util.LinkedList

@Suppress("NAME_SHADOWING")
class Solution {
    fun pathInZigZagTree(label: Int): List<Int> {
        var label = label
        val answer: MutableList<Int> = LinkedList()
        while (label != 0) {
            answer.add(0, label)
            val logNode = (Math.log(label.toDouble()) / Math.log(2.0)).toInt()
            val levelStart = Math.pow(2.0, logNode.toDouble()).toInt()
            val diff = label - levelStart
            val d2 = diff / 2
            val prevEnd = levelStart - 1
            val prevLabel = prevEnd - d2
            label = prevLabel
        }
        return answer
    }
}
```