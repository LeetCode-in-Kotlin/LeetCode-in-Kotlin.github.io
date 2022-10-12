[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 118\. Pascal's Triangle

Easy

Given an integer `numRows`, return the first numRows of **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** numRows = 5

**Output:** [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]

**Example 2:**

**Input:** numRows = 1

**Output:** [[1]]

**Constraints:**

*   `1 <= numRows <= 30`

## Solution

```kotlin
class Solution {
    fun generate(numRows: Int): List<List<Int>> {
        val output: MutableList<List<Int>> = ArrayList()
        for (i in 0 until numRows) {
            val currRow: MutableList<Int> = ArrayList()
            for (j in 0..i) {
                if (j == 0 || j == i || i <= 1) {
                    currRow.add(1)
                } else {
                    val currCell = output[i - 1][j - 1] + output[i - 1][j]
                    currRow.add(currCell)
                }
            }
            output.add(currRow)
        }
        return output
    }
}
```