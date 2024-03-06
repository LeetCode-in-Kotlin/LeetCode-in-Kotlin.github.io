[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3017\. Count the Number of Houses at a Certain Distance II

Hard

You are given three **positive** integers `n`, `x`, and `y`.

In a city, there exist houses numbered `1` to `n` connected by `n` streets. There is a street connecting the house numbered `i` with the house numbered `i + 1` for all `1 <= i <= n - 1` . An additional street connects the house numbered `x` with the house numbered `y`.

For each `k`, such that `1 <= k <= n`, you need to find the number of **pairs of houses** <code>(house<sub>1</sub>, house<sub>2</sub>)</code> such that the **minimum** number of streets that need to be traveled to reach <code>house<sub>2</sub></code> from <code>house<sub>1</sub></code> is `k`.

Return _a **1-indexed** array_ `result` _of length_ `n` _where_ `result[k]` _represents the **total** number of pairs of houses such that the **minimum** streets required to reach one house from the other is_ `k`.

**Note** that `x` and `y` can be **equal**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/12/20/example2.png)

**Input:** n = 3, x = 1, y = 3

**Output:** [6,0,0]

**Explanation:** Let's look at each pair of houses: 
- For the pair (1, 2), we can go from house 1 to house 2 directly. 
- For the pair (2, 1), we can go from house 2 to house 1 directly. 
- For the pair (1, 3), we can go from house 1 to house 3 directly. 
- For the pair (3, 1), we can go from house 3 to house 1 directly. 
- For the pair (2, 3), we can go from house 2 to house 3 directly. 
- For the pair (3, 2), we can go from house 3 to house 2 directly.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/12/20/example3.png)

**Input:** n = 5, x = 2, y = 4

**Output:** [10,8,2,0,0]

**Explanation:** For each distance k the pairs are: 
- For k == 1, the pairs are (1, 2), (2, 1), (2, 3), (3, 2), (2, 4), (4, 2), (3, 4), (4, 3), (4, 5), and (5, 4). 
- For k == 2, the pairs are (1, 3), (3, 1), (1, 4), (4, 1), (2, 5), (5, 2), (3, 5), and (5, 3). 
- For k == 3, the pairs are (1, 5), and (5, 1). 
- For k == 4 and k == 5, there are no pairs.

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/12/20/example5.png)

**Input:** n = 4, x = 1, y = 1

**Output:** [6,4,2,0]

**Explanation:** For each distance k the pairs are: 
- For k == 1, the pairs are (1, 2), (2, 1), (2, 3), (3, 2), (3, 4), and (4, 3).
- For k == 2, the pairs are (1, 3), (3, 1), (2, 4), and (4, 2).
- For k == 3, the pairs are (1, 4), and (4, 1). 
- For k == 4, there are no pairs.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= x, y <= n`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun countOfPairs(n: Int, x: Int, y: Int): LongArray {
        val result = LongArray(n)
        val leftCount = (min(x, y) - 1)
        val rightCount = (n - max(x, y))
        val circleCount = n - leftCount - rightCount
        circleInternal(circleCount, result)
        lineToCircle(leftCount, circleCount, result)
        lineToCircle(rightCount, circleCount, result)
        lineToLine(leftCount, rightCount, if (x == y) 1 else 2, result)
        lineInternal(leftCount, result)
        lineInternal(rightCount, result)
        return result
    }

    private fun lineToCircle(lineCount: Int, circleCount: Int, curRes: LongArray) {
        val circleLen = circleCount / 2 + 1
        var curModifier = 0
        for (i in 1 until circleLen + lineCount) {
            if (i <= min(lineCount, circleLen)) {
                curModifier += 4
            } else if (i > max(lineCount, circleLen)) {
                curModifier -= 4
            }
            curRes[i - 1] += curModifier.toLong()
            if (i <= lineCount) {
                curRes[i - 1] = curRes[i - 1] - 2
            }
            if (i >= circleLen && circleCount % 2 == 0) {
                curRes[i - 1] = curRes[i - 1] - 2
            }
        }
    }

    private fun lineToLine(lineCount1: Int, lineCount2: Int, initialDis: Int, curRes: LongArray) {
        var curModifier = 0
        for (i in 1 until lineCount1 + lineCount2) {
            if (i <= min(lineCount1, lineCount2)) {
                curModifier += 2
            } else if (i > max(lineCount1, lineCount2)) {
                curModifier -= 2
            }
            curRes[i - 1 + initialDis] += curModifier.toLong()
        }
    }

    private fun lineInternal(lineCount: Int, curRes: LongArray) {
        for (i in 1 until lineCount) {
            curRes[i - 1] += (lineCount - i) * 2L
        }
    }

    private fun circleInternal(circleCount: Int, curRes: LongArray) {
        for (i in 0 until circleCount / 2) {
            if (circleCount % 2 == 0 && i + 1 == circleCount / 2) {
                curRes[i] += circleCount.toLong()
            } else {
                curRes[i] += circleCount * 2L
            }
        }
    }
}
```