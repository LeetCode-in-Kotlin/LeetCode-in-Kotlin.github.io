[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3725\. Count Ways to Choose Coprime Integers from Rows

Hard

You are given a `m x n` matrix `mat` of positive integers.

Return an integer denoting the number of ways to choose **exactly one** integer from each row of `mat` such that the **greatest common divisor** of all chosen integers is 1.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** mat = \[\[1,2],[3,4]]

**Output:** 3

**Explanation:**

| Chosen integer in the first row | Chosen integer in the second row | Greatest common divisor of chosen integers |
|---------------------------------|----------------------------------|--------------------------------------------|
| 1 | 3 | 1 |
| 1 | 4 | 1 |
| 2 | 3 | 1 |
| 2 | 4 | 2 |

3 of these combinations have a greatest common divisor of 1. Therefore, the answer is 3.

**Example 2:**

**Input:** mat = \[\[2,2],[2,2]]

**Output:** 0

**Explanation:**

Every combination has a greatest common divisor of 2. Therefore, the answer is 0.

**Constraints:**

*   `1 <= m == mat.length <= 150`
*   `1 <= n == mat[i].length <= 150`
*   `1 <= mat[i][j] <= 150`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun countCoprime(mat: Array<IntArray>): Int {
        val m = mat.size
        val n = mat[0].size
        var maxVal = 0
        for (ints in mat) {
            for (j in 0..<n) {
                maxVal = max(maxVal, ints[j])
            }
        }
        val gcdWays: MutableMap<Int, Long> = HashMap()
        for (g in maxVal downTo 1) {
            var ways = countWaysWithDivisor(mat, g, m, n)
            if (ways > 0) {
                var multiple = 2 * g
                while (multiple <= maxVal) {
                    if (gcdWays.containsKey(multiple)) {
                        ways = (ways - gcdWays[multiple]!! + MOD) % MOD
                    }
                    multiple += g
                }
                gcdWays[g] = ways
            }
        }
        return gcdWays.getOrDefault(1, 0L).toInt()
    }

    private fun countWaysWithDivisor(matrix: Array<IntArray>, divisor: Int, rows: Int, cols: Int): Long {
        var totalWays: Long = 1
        for (row in 0..<rows) {
            var validChoices = 0
            for (col in 0..<cols) {
                if (matrix[row][col] % divisor == 0) {
                    validChoices++
                }
            }
            if (validChoices == 0) {
                return 0
            }
            totalWays = (totalWays * validChoices) % MOD
        }
        return totalWays
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```