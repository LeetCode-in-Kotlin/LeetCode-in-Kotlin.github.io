[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3445\. Maximum Difference Between Even and Odd Frequency II

Hard

You are given a string `s` and an integer `k`. Your task is to find the **maximum** difference between the frequency of **two** characters, `freq[a] - freq[b]`, in a **substring** `subs` of `s`, such that:

*   `subs` has a size of **at least** `k`.
*   Character `a` has an _odd frequency_ in `subs`.
*   Character `b` has an _even frequency_ in `subs`.

Return the **maximum** difference.

**Note** that `subs` can contain more than 2 **distinct** characters.

**Example 1:**

**Input:** s = "12233", k = 4

**Output:** \-1

**Explanation:**

For the substring `"12233"`, the frequency of `'1'` is 1 and the frequency of `'3'` is 2. The difference is `1 - 2 = -1`.

**Example 2:**

**Input:** s = "1122211", k = 3

**Output:** 1

**Explanation:**

For the substring `"11222"`, the frequency of `'2'` is 3 and the frequency of `'1'` is 2. The difference is `3 - 2 = 1`.

**Example 3:**

**Input:** s = "110", k = 3

**Output:** \-1

**Constraints:**

*   <code>3 <= s.length <= 3 * 10<sup>4</sup></code>
*   `s` consists only of digits `'0'` to `'4'`.
*   The input is generated that at least one substring has a character with an even frequency and a character with an odd frequency.
*   `1 <= k <= s.length`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxDifference(s: String, k: Int): Int {
        val n = s.length
        val pre = Array<IntArray>(5) { IntArray(n) }
        val closestRight = Array<IntArray>(5) { IntArray(n) }
        for (x in 0..4) {
            closestRight[x].fill(n)
            for (i in 0..<n) {
                val num = s[i].code - '0'.code
                pre[x][i] = if (num == x) 1 else 0
                if (i > 0) {
                    pre[x][i] += pre[x][i - 1]
                }
            }
            for (i in n - 1 downTo 0) {
                val num = s[i].code - '0'.code
                if (i < n - 1) {
                    closestRight[x][i] = closestRight[x][i + 1]
                }
                if (num == x) {
                    closestRight[x][i] = i
                }
            }
        }
        var ans = Int.Companion.MIN_VALUE
        for (a in 0..4) {
            for (b in 0..4) {
                if (a != b) {
                    ans = max(ans, go(k, a, b, pre, closestRight, n))
                }
            }
        }
        return ans
    }

    private fun go(k: Int, odd: Int, even: Int, pre: Array<IntArray>, closestRight: Array<IntArray>, n: Int): Int {
        val suf: Array<Array<IntArray>> = Array<Array<IntArray>>(2) { Array<IntArray>(2) { IntArray(n) } }
        for (arr2D in suf) {
            for (arr in arr2D) {
                arr.fill(Int.Companion.MIN_VALUE)
            }
        }
        for (endIdx in 0..<n) {
            val oddParity = pre[odd][endIdx] % 2
            val evenParity = pre[even][endIdx] % 2
            if (pre[odd][endIdx] > 0 && pre[even][endIdx] > 0) {
                suf[oddParity][evenParity][endIdx] = pre[odd][endIdx] - pre[even][endIdx]
            }
        }
        for (oddParity in 0..1) {
            for (evenParity in 0..1) {
                for (endIdx in n - 2 downTo 0) {
                    suf[oddParity][evenParity][endIdx] = max(
                        suf[oddParity][evenParity][endIdx],
                        suf[oddParity][evenParity][endIdx + 1],
                    )
                }
            }
        }
        var ans = Int.Companion.MIN_VALUE
        for (startIdx in 0..<n) {
            val minEndIdx = startIdx + k - 1
            if (minEndIdx >= n) {
                break
            }
            val oddBelowI = (if (startIdx == 0) 0 else pre[odd][startIdx - 1])
            val evenBelowI = (if (startIdx == 0) 0 else pre[even][startIdx - 1])
            val goodOddParity = (oddBelowI + 1) % 2
            val goodEvenParity = evenBelowI % 2
            val query = max(
                max((startIdx + k - 1), closestRight[odd][startIdx]),
                closestRight[even][startIdx],
            )
            if (query >= n) {
                continue
            }
            val `val` = suf[goodOddParity][goodEvenParity][query]
            if (`val` == Int.Companion.MIN_VALUE) {
                continue
            }
            ans = max(ans, (`val` - oddBelowI + evenBelowI))
        }
        return ans
    }
}
```