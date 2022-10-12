[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 119\. Pascal's Triangle II

Easy

Given an integer `rowIndex`, return the <code>rowIndex<sup>th</sup></code> (**0-indexed**) row of the **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** rowIndex = 3

**Output:** [1,3,3,1]

**Example 2:**

**Input:** rowIndex = 0

**Output:** [1]

**Example 3:**

**Input:** rowIndex = 1

**Output:** [1,1]

**Constraints:**

*   `0 <= rowIndex <= 33`

**Follow up:** Could you optimize your algorithm to use only `O(rowIndex)` extra space?

## Solution

```kotlin
class Solution {
    fun getRow(rowIndex: Int): List<Int> {
        val buffer = IntArray(rowIndex + 1)
        buffer[0] = 1
        computeRow(buffer, 1)
        // Copy buffer to List of Integer.
        val ans: MutableList<Int> = ArrayList(buffer.size)
        for (j in buffer) {
            ans.add(j)
        }
        return ans
    }

    private fun computeRow(buffer: IntArray, k: Int) {
        if (k >= buffer.size) {
            return
        }
        var previous = buffer[0]
        for (i in 1 until k) {
            val tmp = previous + buffer[i]
            previous = buffer[i]
            buffer[i] = tmp
        }
        buffer[k] = 1
        computeRow(buffer, k + 1)
    }
}
```