[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3518\. Smallest Palindromic Rearrangement II

Hard

You are given a **palindromic** string `s` and an integer `k`.

Return the **k-th** **lexicographically smallest** palindromic permutation of `s`. If there are fewer than `k` distinct palindromic permutations, return an empty string.

**Note:** Different rearrangements that yield the same palindromic string are considered identical and are counted once.

**Example 1:**

**Input:** s = "abba", k = 2

**Output:** "baab"

**Explanation:**

*   The two distinct palindromic rearrangements of `"abba"` are `"abba"` and `"baab"`.
*   Lexicographically, `"abba"` comes before `"baab"`. Since `k = 2`, the output is `"baab"`.

**Example 2:**

**Input:** s = "aa", k = 2

**Output:** ""

**Explanation:**

*   There is only one palindromic rearrangement: `"aa"`.
*   The output is an empty string since `k = 2` exceeds the number of possible rearrangements.

**Example 3:**

**Input:** s = "bacab", k = 1

**Output:** "abcba"

**Explanation:**

*   The two distinct palindromic rearrangements of `"bacab"` are `"abcba"` and `"bacab"`.
*   Lexicographically, `"abcba"` comes before `"bacab"`. Since `k = 1`, the output is `"abcba"`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.
*   `s` is guaranteed to be palindromic.
*   <code>1 <= k <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun smallestPalindrome(inputStr: String, k: Int): String {
        var k = k
        val frequency = IntArray(26)
        for (i in 0..<inputStr.length) {
            val ch = inputStr[i]
            frequency[ch.code - 'a'.code]++
        }
        var mid = 0.toChar()
        for (i in 0..25) {
            if (frequency[i] % 2 == 1) {
                mid = ('a'.code + i).toChar()
                frequency[i]--
                break
            }
        }
        val halfFreq = IntArray(26)
        var halfLength = 0
        for (i in 0..25) {
            halfFreq[i] = frequency[i] / 2
            halfLength += halfFreq[i]
        }
        val totalPerms = multinomial(halfFreq)
        if (k > totalPerms) {
            return ""
        }
        val firstHalfBuilder = StringBuilder()
        for (i in 0..<halfLength) {
            for (c in 0..25) {
                if (halfFreq[c] > 0) {
                    halfFreq[c]--
                    val perms = multinomial(halfFreq)
                    if (perms >= k) {
                        firstHalfBuilder.append(('a'.code + c).toChar())
                        break
                    } else {
                        k -= perms.toInt()
                        halfFreq[c]++
                    }
                }
            }
        }
        val firstHalf = firstHalfBuilder.toString()
        val revHalf = StringBuilder(firstHalf).reverse().toString()
        return if (mid.code == 0) {
            firstHalf + revHalf
        } else {
            firstHalf + mid + revHalf
        }
    }

    private fun multinomial(counts: IntArray): Long {
        var tot = 0
        for (cnt in counts) {
            tot += cnt
        }
        var res: Long = 1
        for (i in 0..25) {
            val cnt = counts[i]
            res = res * binom(tot, cnt)
            if (res >= MAX_K) {
                return MAX_K
            }
            tot -= cnt
        }
        return res
    }

    private fun binom(n: Int, k: Int): Long {
        var k = k
        if (k > n) {
            return 0
        }
        if (k > n - k) {
            k = n - k
        }
        var result: Long = 1
        for (i in 1..k) {
            result = result * (n - i + 1) / i
            if (result >= MAX_K) {
                return MAX_K
            }
        }
        return result
    }

    companion object {
        private const val MAX_K: Long = 1000001
    }
}
```