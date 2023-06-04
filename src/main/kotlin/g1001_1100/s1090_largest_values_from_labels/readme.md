[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1090\. Largest Values From Labels

Medium

There is a set of `n` items. You are given two integer arrays `values` and `labels` where the value and the label of the <code>i<sup>th</sup></code> element are `values[i]` and `labels[i]` respectively. You are also given two integers `numWanted` and `useLimit`.

Choose a subset `s` of the `n` elements such that:

*   The size of the subset `s` is **less than or equal to** `numWanted`.
*   There are **at most** `useLimit` items with the same label in `s`.

The **score** of a subset is the sum of the values in the subset.

Return _the maximum **score** of a subset_ `s`.

**Example 1:**

**Input:** values = [5,4,3,2,1], labels = [1,1,2,2,3], numWanted = 3, useLimit = 1

**Output:** 9

**Explanation:** The subset chosen is the first, third, and fifth items.

**Example 2:**

**Input:** values = [5,4,3,2,1], labels = [1,3,3,3,2], numWanted = 3, useLimit = 2

**Output:** 12

**Explanation:** The subset chosen is the first, second, and third items.

**Example 3:**

**Input:** values = [9,8,8,7,6], labels = [0,0,0,1,1], numWanted = 3, useLimit = 1

**Output:** 16

**Explanation:** The subset chosen is the first and fourth items.

**Constraints:**

*   `n == values.length == labels.length`
*   <code>1 <= n <= 2 * 10<sup>4</sup></code>
*   <code>0 <= values[i], labels[i] <= 2 * 10<sup>4</sup></code>
*   `1 <= numWanted, useLimit <= n`

## Solution

```kotlin
import java.util.PriorityQueue

@Suppress("NAME_SHADOWING")
class Solution {
    private class Node(var `val`: Int, var label: Int)

    fun largestValsFromLabels(values: IntArray, labels: IntArray, numWanted: Int, useLimit: Int): Int {
        var numWanted = numWanted
        val maxHeap =
            PriorityQueue { a: Node, b: Node -> if (b.`val` != a.`val`) b.`val` - a.`val` else a.label - b.label }
        val n = values.size
        for (i in 0 until n) {
            maxHeap.offer(Node(values[i], labels[i]))
        }
        var ans = 0
        val labelAddedCount: HashMap<Int, Int> = HashMap()
        while (maxHeap.isNotEmpty() && numWanted > 0) {
            val cur = maxHeap.poll()
            if (labelAddedCount.containsKey(cur.label) &&
                labelAddedCount[cur.label]!! >= useLimit
            ) {
                continue
            }
            if (cur.`val` > 0) {
                ans += cur.`val`
                labelAddedCount[cur.label] = labelAddedCount.getOrDefault(cur.label, 0) + 1
                numWanted--
            }
        }
        return ans
    }
}
```