[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 552\. Student Attendance Record II

Hard

An attendance record for a student can be represented as a string where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

*   `'A'`: Absent.
*   `'L'`: Late.
*   `'P'`: Present.

Any student is eligible for an attendance award if they meet **both** of the following criteria:

*   The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
*   The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Given an integer `n`, return _the **number** of possible attendance records of length_ `n` _that make a student eligible for an attendance award. The answer may be very large, so return it **modulo**_ <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 2

**Output:** 8

**Explanation:** There are 8 records with length 2 that are eligible for an award: "PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL" Only "AA" is not eligible because there are 2 absences (there need to be fewer than 2).

**Example 2:**

**Input:** n = 1

**Output:** 3

**Example 3:**

**Input:** n = 10101

**Output:** 183236316

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun checkRecord(n: Int): Int {
        if (n == 0 || n == 1 || n == 2) {
            return n * 3 + n * (n - 1)
        }
        val mod: Long = 1000000007
        val matrix = arrayOf(
            longArrayOf(1, 1, 0, 1, 0, 0),
            longArrayOf(1, 0, 1, 1, 0, 0),
            longArrayOf(1, 0, 0, 1, 0, 0),
            longArrayOf(0, 0, 0, 1, 1, 0),
            longArrayOf(0, 0, 0, 1, 0, 1),
            longArrayOf(0, 0, 0, 1, 0, 0),
        )
        val e = quickPower(matrix, n - 1)
        return (
            (e[0].sum() + e[1].sum() + e[3].sum()) %
                mod
            ).toInt()
    }

    private fun quickPower(a: Array<LongArray>, times: Int): Array<LongArray> {
        var a = a
        var times = times
        val n = a.size
        var e = Array(n) { LongArray(n) }
        for (i in 0 until n) {
            e[i][i] = 1
        }
        while (times != 0) {
            if (times % 2 == 1) {
                e = multiple(e, a, n)
            }
            times = times shr 1
            a = multiple(a, a, n)
        }
        return e
    }

    private fun multiple(a: Array<LongArray>, b: Array<LongArray>, n: Int): Array<LongArray> {
        val target = Array(n) { LongArray(n) }
        for (j in 0 until n) {
            for (k in 0 until n) {
                for (l in 0 until n) {
                    target[j][k] += a[j][l] * b[l][k]
                    target[j][k] = target[j][k] % 1000000007
                }
            }
        }
        return target
    }
}
```