[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3480\. Maximize Subarrays After Removing One Conflicting Pair

Hard

You are given an integer `n` which represents an array `nums` containing the numbers from 1 to `n` in order. Additionally, you are given a 2D array `conflictingPairs`, where `conflictingPairs[i] = [a, b]` indicates that `a` and `b` form a conflicting pair.

Remove **exactly** one element from `conflictingPairs`. Afterward, count the number of non-empty subarrays of `nums` which do not contain both `a` and `b` for any remaining conflicting pair `[a, b]`.

Return the **maximum** number of subarrays possible after removing **exactly** one conflicting pair.

**Example 1:**

**Input:** n = 4, conflictingPairs = \[\[2,3],[1,4]]

**Output:** 9

**Explanation:**

*   Remove `[2, 3]` from `conflictingPairs`. Now, `conflictingPairs = \[\[1, 4]]`.
*   There are 9 subarrays in `nums` where `[1, 4]` do not appear together. They are `[1]`, `[2]`, `[3]`, `[4]`, `[1, 2]`, `[2, 3]`, `[3, 4]`, `[1, 2, 3]` and `[2, 3, 4]`.
*   The maximum number of subarrays we can achieve after removing one element from `conflictingPairs` is 9.

**Example 2:**

**Input:** n = 5, conflictingPairs = \[\[1,2],[2,5],[3,5]]

**Output:** 12

**Explanation:**

*   Remove `[1, 2]` from `conflictingPairs`. Now, `conflictingPairs = \[\[2, 5], [3, 5]]`.
*   There are 12 subarrays in `nums` where `[2, 5]` and `[3, 5]` do not appear together.
*   The maximum number of subarrays we can achieve after removing one element from `conflictingPairs` is 12.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `1 <= conflictingPairs.length <= 2 * n`
*   `conflictingPairs[i].length == 2`
*   `1 <= conflictingPairs[i][j] <= n`
*   `conflictingPairs[i][0] != conflictingPairs[i][1]`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxSubarrays(n: Int, conflictingPairs: Array<IntArray>): Long {
        val totalSubarrays = n.toLong() * (n + 1) / 2
        val h = IntArray(n + 1)
        val d2 = IntArray(n + 1)
        h.fill(n + 1)
        d2.fill(n + 1)
        for (pair in conflictingPairs) {
            var a = pair[0]
            var b = pair[1]
            if (a > b) {
                val temp = a
                a = b
                b = temp
            }
            if (b < h[a]) {
                d2[a] = h[a]
                h[a] = b
            } else if (b < d2[a]) {
                d2[a] = b
            }
        }
        val f = IntArray(n + 2)
        f[n + 1] = n + 1
        f[n] = h[n]
        for (i in n - 1 downTo 1) {
            f[i] = min(h[i], f[i + 1])
        }
        // forbiddenCount(x) returns (n - x + 1) if x <= n, else 0.
        // This is the number of forbidden subarrays starting at some i when f[i] = x.
        var originalUnion: Long = 0
        for (i in 1..n) {
            if (f[i] <= n) {
                originalUnion += (n - f[i] + 1).toLong()
            }
        }
        val originalValid = totalSubarrays - originalUnion
        var best = originalValid
        // For each index j (1 <= j <= n) where a candidate conflicting pair exists,
        // simulate removal of the pair that gave h[j] (if any).
        // (If there is no candidate pair at j, h[j] remains n+1.)
        for (j in 1..n) {
            // no conflicting pair at index j
            if (h[j] == n + 1) {
                continue
            }
            // Simulate removal: new candidate at j becomes d2[j]
            val newCandidate = if (j < n) min(d2[j], f[j + 1]) else d2[j]
            // We'll recompute the new f values for indices 1..j.
            // Let newF[i] denote the updated value.
            // For i > j, newF[i] remains as original f[i].
            // For i = j, newF[j] = min( newCandidate, f[j+1] ) (which is newCandidate by
            // definition).
            val newFj = newCandidate
            // forbiddenCount(x) is defined as (n - x + 1) if x<= n, else 0.
            var delta = forbiddenCount(newFj, n) - forbiddenCount(f[j], n)
            var cur = newFj
            // Now update backwards for i = j-1 down to 1.
            for (i in j - 1 downTo 1) {
                val newVal = min(h[i], cur)
                // no further change for i' <= i
                if (newVal == f[i]) {
                    break
                }
                delta += forbiddenCount(newVal, n) - forbiddenCount(f[i], n)
                cur = newVal
            }
            val newUnion = originalUnion + delta
            val newValid = totalSubarrays - newUnion
            best = max(best, newValid)
        }
        return best
    }

    private fun forbiddenCount(x: Int, n: Int): Long {
        return (if (x <= n) (n - x + 1) else 0).toLong()
    }
}
```