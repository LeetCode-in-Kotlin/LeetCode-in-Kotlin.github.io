[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3193\. Count the Number of Inversions

Hard

You are given an integer `n` and a 2D array `requirements`, where <code>requirements[i] = [end<sub>i</sub>, cnt<sub>i</sub>]</code> represents the end index and the **inversion** count of each requirement.

A pair of indices `(i, j)` from an integer array `nums` is called an **inversion** if:

*   `i < j` and `nums[i] > nums[j]`

Return the number of permutations `perm` of `[0, 1, 2, ..., n - 1]` such that for **all** `requirements[i]`, <code>perm[0..end<sub>i</sub>]</code> has exactly <code>cnt<sub>i</sub></code> inversions.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, requirements = \[\[2,2],[0,0]]

**Output:** 2

**Explanation:**

The two permutations are:

*   `[2, 0, 1]`
    *   Prefix `[2, 0, 1]` has inversions `(0, 1)` and `(0, 2)`.
    *   Prefix `[2]` has 0 inversions.
*   `[1, 2, 0]`
    *   Prefix `[1, 2, 0]` has inversions `(0, 2)` and `(1, 2)`.
    *   Prefix `[1]` has 0 inversions.

**Example 2:**

**Input:** n = 3, requirements = \[\[2,2],[1,1],[0,0]]

**Output:** 1

**Explanation:**

The only satisfying permutation is `[2, 0, 1]`:

*   Prefix `[2, 0, 1]` has inversions `(0, 1)` and `(0, 2)`.
*   Prefix `[2, 0]` has an inversion `(0, 1)`.
*   Prefix `[2]` has 0 inversions.

**Example 3:**

**Input:** n = 2, requirements = \[\[0,0],[1,0]]

**Output:** 1

**Explanation:**

The only satisfying permutation is `[0, 1]`:

*   Prefix `[0]` has 0 inversions.
*   Prefix `[0, 1]` has an inversion `(0, 1)`.

**Constraints:**

*   `2 <= n <= 300`
*   `1 <= requirements.length <= n`
*   <code>requirements[i] = [end<sub>i</sub>, cnt<sub>i</sub>]</code>
*   <code>0 <= end<sub>i</sub> <= n - 1</code>
*   <code>0 <= cnt<sub>i</sub> <= 400</code>
*   The input is generated such that there is at least one `i` such that <code>end<sub>i</sub> == n - 1</code>.
*   The input is generated such that all <code>end<sub>i</sub></code> are unique.

## Solution

```kotlin
class Solution {
    fun numberOfPermutations(n: Int, r: Array<IntArray>): Int {
        r.sortWith { o1: IntArray, o2: IntArray -> o1[0] - o2[0] }
        if (r[0][0] == 0 && r[0][1] > 0) {
            return 0
        }
        var ri = if (r[0][0] == 0) 1 else 0
        var a: Long = 1
        var t: Long
        val m = Array(n) { IntArray(401) }
        m[0][0] = 1
        for (i in 1 until m.size) {
            m[i][0] = m[i - 1][0]
            for (j in 1..i) {
                m[i][j] = (m[i][j] + m[i][j - 1]) % MOD
                m[i][j] = (m[i][j] + m[i - 1][j]) % MOD
            }
            for (j in i + 1..r[ri][1]) {
                m[i][j] = (m[i][j] + m[i][j - 1]) % MOD
                m[i][j] = (m[i][j] + m[i - 1][j]) % MOD
                m[i][j] = (m[i][j] - m[i - 1][j - i - 1])
                if (m[i][j] < 0) {
                    m[i][j] += MOD
                }
            }
            if (r[ri][0] == i) {
                t = m[i][r[ri][1]].toLong()
                if (t == 0L) {
                    return 0
                }
                m[i].fill(0)
                m[i][r[ri][1]] = 1
                a = (a * t) % MOD
                ri++
            }
        }
        return a.toInt()
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```