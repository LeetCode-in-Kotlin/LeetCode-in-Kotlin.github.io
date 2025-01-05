[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3395\. Subsequences with a Unique Middle Mode I

Hard

Given an integer array `nums`, find the number of subsequences of size 5 of `nums` with a **unique middle mode**.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **mode** of a sequence of numbers is defined as the element that appears the **maximum** number of times in the sequence.

A sequence of numbers contains a **unique mode** if it has only one mode.

A sequence of numbers `seq` of size 5 contains a **unique middle mode** if the _middle element_ (`seq[2]`) is a **unique mode**.

A **subsequence** is a **non-empty** array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [1,1,1,1,1,1]

**Output:** 6

**Explanation:**

`[1, 1, 1, 1, 1]` is the only subsequence of size 5 that can be formed, and it has a unique middle mode of 1. This subsequence can be formed in 6 different ways, so the output is 6.

**Example 2:**

**Input:** nums = [1,2,2,3,3,4]

**Output:** 4

**Explanation:**

`[1, 2, 2, 3, 4]` and `[1, 2, 3, 3, 4]` each have a unique middle mode because the number at index 2 has the greatest frequency in the subsequence. `[1, 2, 2, 3, 3]` does not have a unique middle mode because 2 and 3 appear twice.

**Example 3:**

**Input:** nums = [0,1,2,3,4,5,6,7,8]

**Output:** 0

**Explanation:**

There is no subsequence of length 5 with a unique middle mode.

**Constraints:**

*   `5 <= nums.length <= 1000`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun subsequencesWithMiddleMode(a: IntArray): Int {
        val n = a.size
        // Create a dictionary to store indices of each number
        val dict: MutableMap<Int, MutableList<Int>> = HashMap()
        for (i in 0..<n) {
            dict.computeIfAbsent(a[i]) { _: Int -> ArrayList<Int>() }.add(i)
        }
        var ans = 0L
        // Iterate over each unique number and its indices
        for (entry in dict.entries) {
            val b: MutableList<Int> = entry.value
            val m = b.size
            for (k in 0..<m) {
                val i: Int = b[k]
                val r = m - 1 - k
                val u = i - k
                val v = (n - 1 - i) - r
                // Case 2: Frequency of occurrence is 2 times
                ans = (
                    ans + convert(k, 1) * convert(u, 1) % MOD * convert(
                        v,
                        2,
                    ) % MOD
                    ) % MOD
                ans = (
                    ans + convert(r, 1) * convert(u, 2) % MOD * convert(
                        v,
                        1,
                    ) % MOD
                    ) % MOD
                // Case 3: Frequency of occurrence is 3 times
                ans = (ans + convert(k, 2) * convert(v, 2) % MOD) % MOD
                ans = (ans + convert(r, 2) * convert(u, 2) % MOD) % MOD
                ans =
                    (
                        (
                            ans +
                                convert(k, 1) *
                                convert(r, 1) %
                                MOD
                                * convert(u, 1) %
                                MOD
                                * convert(v, 1) %
                                MOD
                            ) %
                            MOD
                        )
                // Case 4: Frequency of occurrence is 4 times
                ans = (
                    ans + convert(k, 2) * convert(r, 1) % MOD * convert(
                        v,
                        1,
                    ) % MOD
                    ) % MOD
                ans = (
                    ans + convert(k, 1) * convert(r, 2) % MOD * convert(
                        u,
                        1,
                    ) % MOD
                    ) % MOD

                // Case 5: Frequency of occurrence is 5 times
                ans = (ans + convert(k, 2) * convert(r, 2) % MOD) % MOD
            }
        }
        var dif: Long = 0
        // Principle of inclusion-exclusion
        for (midEntry in dict.entries) {
            val b: MutableList<Int> = midEntry.value
            val m = b.size
            for (tmpEntry in dict.entries) {
                if (midEntry.key != tmpEntry.key) {
                    val c: MutableList<Int> = tmpEntry.value
                    val size = c.size
                    var k = 0
                    var j = 0
                    while (k < m) {
                        val i: Int = b[k]
                        val r = m - 1 - k
                        val u = i - k
                        val v = (n - 1 - i) - r
                        while (j < size && c[j] < i) {
                            j++
                        }
                        val x = j
                        val y = size - x
                        dif =
                            (
                                (
                                    dif +
                                        convert(k, 1) *
                                        convert(x, 1) %
                                        MOD
                                        * convert(y, 1) %
                                        MOD
                                        * convert(v - y, 1) %
                                        MOD
                                    ) %
                                    MOD
                                )
                        dif =
                            (
                                (
                                    dif +
                                        convert(k, 1) *
                                        convert(y, 2) %
                                        MOD
                                        * convert(u - x, 1) %
                                        MOD
                                    ) %
                                    MOD
                                )
                        dif =
                            (
                                (
                                    dif + convert(k, 1) * convert(x, 1) % MOD * convert(
                                        y,
                                        2,
                                    ) % MOD
                                    ) %
                                    MOD
                                )

                        dif =
                            (
                                (
                                    dif +
                                        convert(r, 1) *
                                        convert(x, 1) %
                                        MOD
                                        * convert(y, 1) %
                                        MOD
                                        * convert(u - x, 1) %
                                        MOD
                                    ) %
                                    MOD
                                )
                        dif =
                            (
                                (
                                    dif +
                                        convert(r, 1) *
                                        convert(x, 2) %
                                        MOD
                                        * convert(v - y, 1) %
                                        MOD
                                    ) %
                                    MOD
                                )
                        dif =
                            (
                                (
                                    dif + convert(r, 1) * convert(x, 2) % MOD * convert(
                                        y,
                                        1,
                                    ) % MOD
                                    ) %
                                    MOD
                                )
                        k++
                    }
                }
            }
        }
        return ((ans - dif + MOD) % MOD).toInt()
    }

    private fun convert(n: Int, k: Int): Long {
        if (k > n) {
            return 0
        }
        if (k == 0 || k == n) {
            return 1
        }
        var res: Long = 1
        for (i in 0..<k) {
            res = res * (n - i) / (i + 1)
        }
        return res % MOD
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```