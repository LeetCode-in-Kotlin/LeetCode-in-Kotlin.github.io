[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3883\. Count Non Decreasing Arrays With Given Digit Sums

Hard

You are given an integer array `digitSum` of length `n`.

An array `arr` of length `n` is considered **valid** if:

*   `0 <= arr[i] <= 5000`
*   it is **non-decreasing**.
*   the **sum of the digits** of `arr[i]` **equals** `digitSum[i]`.

Return an integer denoting the number of **distinct valid arrays**. Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

An array is said to be **non-decreasing** if each element is greater than or equal to the previous element, if it exists.

**Example 1:**

**Input:** digitSum = [25,1]

**Output:** 6

**Explanation:**

Numbers whose sum of digits is 25 are 799, 889, 898, 979, 988, and 997.

The only number whose sum of digits is 1 that can appear after these values while keeping the array non-decreasing is 1000.

Thus, the valid arrays are `[799, 1000]`, `[889, 1000]`, `[898, 1000]`, `[979, 1000]`, `[988, 1000]`, and `[997, 1000]`.

Hence, the answer is 6.

**Example 2:**

**Input:** digitSum = [1]

**Output:** 4

**Explanation:**

The valid arrays are `[1]`, `[10]`, `[100]`, and `[1000]`.

Thus, the answer is 4.

**Example 3:**

**Input:** digitSum = [2,49,23]

**Output:** 0

**Explanation:**

There is no integer in the range [0, 5000] whose sum of digits is 49. Thus, the answer is 0.

**Constraints:**

*   `1 <= digitSum.length <= 1000`
*   `0 <= digitSum[i] <= 50`

## Solution

```kotlin
class Solution {
    private fun s(x: Int): Int {
        var x = x
        var r = 0
        while (x > 0) {
            r += x % 10
            x /= 10
        }
        return r
    }

    fun countArrays(d: IntArray): Int {
        val g = buildGroups()
        if (g[d[0]].isEmpty()) {
            return 0
        }
        var dp = createInitialDp(g[d[0]].size)
        for (i in 1..<d.size) {
            dp = transition(dp, g[d[i - 1]], g[d[i]])
            if (dp.isEmpty()) {
                return 0
            }
        }
        return sum(dp)
    }

    private fun buildGroups(): Array<ArrayList<Int>> {
        val g: Array<ArrayList<Int>> = Array<ArrayList<Int>>(51) { ArrayList() }
        for (i in 0..50) {
            g[i] = ArrayList<Int>()
        }
        for (i in 0..5000) {
            g[s(i)].add(i)
        }
        return g
    }

    private fun createInitialDp(size: Int): LongArray {
        val dp = LongArray(size)
        dp.fill(1)
        return dp
    }

    private fun transition(dp: LongArray, previous: ArrayList<Int>, current: ArrayList<Int>): LongArray {
        if (current.isEmpty()) {
            return LongArray(0)
        }
        val prefix = buildPrefixSums(dp)
        val next = LongArray(current.size)
        var k = 0
        for (j in current.indices) {
            while (k < previous.size && previous[k] <= current[j]) {
                k++
            }
            if (k > 0) {
                next[j] = prefix[k - 1]
            }
        }
        return next
    }

    private fun buildPrefixSums(values: LongArray): LongArray {
        val prefix = LongArray(values.size)
        prefix[0] = values[0]
        for (i in 1..<values.size) {
            prefix[i] = (prefix[i - 1] + values[i]) % M
        }
        return prefix
    }

    private fun sum(values: LongArray): Int {
        var result: Long = 0
        for (value in values) {
            result = (result + value) % M
        }
        return result.toInt()
    }

    companion object {
        private const val M = 1000000007
    }
}
```