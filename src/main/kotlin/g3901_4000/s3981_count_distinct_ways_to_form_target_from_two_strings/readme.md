[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3981\. Count Distinct Ways to Form Target from Two Strings

Hard

You are given three strings `word1`, `word2`, and `target`.

Your task is to count the number of ways to form `target` by choosing characters from `word1` and `word2` under the following conditions:

*   For each character of `target`, choose one matching character from either `word1` or `word2`.
*   The chosen indices from `word1` must be **strictly** increasing.
*   The chosen indices from `word2` must be **strictly** increasing.
*   **At least** one character must be chosen from **both** `word1` and `word2`.

Two ways are considered different if, for **at least** one position in `target`, the chosen character comes from a different string or a different index.

Return the number of ways. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** word1 = "abc", word2 = "bac", target = "abc"

**Output:** 5

**Explanation:**

There are 5 ways to form `target`:

*   `word1[0] = 'a'`, `word1[1] = 'b'`, `word2[2] = 'c'`
*   `word1[0] = 'a'`, `word2[0] = 'b'`, `word1[2] = 'c'`
*   `word1[0] = 'a'`, `word2[0] = 'b'`, `word2[2] = 'c'`
*   `word2[1] = 'a'`, `word1[1] = 'b'`, `word1[2] = 'c'`
*   `word2[1] = 'a'`, `word1[1] = 'b'`, `word2[2] = 'c'`

All ways preserve the increasing index order inside each string and choose at least one character from each string.

**Example 2:**

**Input:** word1 = "cd", word2 = "cd", target = "ccd"

**Output:** 4

**Explanation:**

There are 4 ways to form `target`:

*   `word1[0] = 'c'`, `word2[0] = 'c'`, `word1[1] = 'd'`
*   `word1[0] = 'c'`, `word2[0] = 'c'`, `word2[1] = 'd'`
*   `word2[0] = 'c'`, `word1[0] = 'c'`, `word1[1] = 'd'`
*   `word2[0] = 'c'`, `word1[0] = 'c'`, `word2[1] = 'd'`

The first two `'c'` characters in `target` must come one from each string. The final `'d'` can be chosen from either string.

**Example 3:**

**Input:** word1 = "xy", word2 = "xy", target = "xyxy"

**Output:** 2

**Explanation:**

There are 2 ways to form `target`:

*   `word1[0] = 'x'`, `word1[1] = 'y'`, `word2[0] = 'x'`, `word2[1] = 'y'`
*   `word2[0] = 'x'`, `word2[1] = 'y'`, `word1[0] = 'x'`, `word1[1] = 'y'`

Each `"xy"` part in `target` comes entirely from one string.

**Example 4:**

**Input:** word1 = "ab", word2 = "cde", target = "ace"

**Output:** 1

**Explanation:**

The only way is to choose `word1[0] = 'a'`, `word2[0] = 'c'`, and `word2[2] = 'e'`. Thus, the answer is 1.

**Constraints:**

*   `1 <= word1.length, word2.length, target.length <= 100`
*   `word1`, `word2`, and `target` consist of lowercase English letters only.

## Solution

```kotlin
class Solution {
    fun interleaveCharacters(word1: String, word2: String, target: String): Int {
        val wl1 = word1.length
        val wl2 = word2.length
        val tl = target.length
        var dp = initializeDP(wl1, wl2)
        var nextDp = Array<IntArray>(wl1 + 1) { IntArray(wl2 + 1) }
        val s1: Array<IntArray> = Array<IntArray>(wl1 + 1) { IntArray(wl2 + 1) }
        val s2 = Array<IntArray>(wl1 + 1) { IntArray(wl2 + 1) }
        val m1 = BooleanArray(wl1)
        val m2 = BooleanArray(wl2)
        for (i in tl - 1 downTo 0) {
            val tc = target[i]
            updateMatches(word1, m1, tc)
            updateMatches(word2, m2, tc)
            calculateS1(s1, dp, m1, wl1, wl2)
            calculateS2(s2, s1, nextDp, dp, m2, wl1, wl2)
            val temp = dp
            dp = nextDp
            nextDp = temp
        }
        val dp1 = helper(word1, target, tl, wl1)
        val dp2 = helper(word2, target, tl, wl2)
        val ans: Long = (dp[0][0] - dp1[0] - dp2[0]) % MOD
        return ((ans + MOD) % MOD).toInt()
    }

    private fun initializeDP(wl1: Int, wl2: Int): Array<IntArray> {
        val dp = Array<IntArray>(wl1 + 1) { IntArray(wl2 + 1) }
        for (i in 0..wl1) {
            dp[i].fill(1)
        }
        return dp
    }

    private fun updateMatches(word: String, matches: BooleanArray, targetChar: Char) {
        for (i in 0..<word.length) {
            matches[i] = word[i] == targetChar
        }
    }

    private fun calculateS1(s1: Array<IntArray>, dp: Array<IntArray>, m1: BooleanArray, wl1: Int, wl2: Int) {
        s1[wl1].fill(0)
        for (j in wl1 - 1 downTo 0) {
            val current = s1[j]
            val next = s1[j + 1]
            if (m1[j]) {
                calculateS1Row(current, next, dp[j + 1], wl2)
            } else {
                System.arraycopy(next, 0, current, 0, wl2 + 1)
            }
        }
    }

    private fun calculateS1Row(current: IntArray, next: IntArray, dpNext: IntArray, wl2: Int) {
        for (k in wl2 downTo 0) {
            current[k] = (next[k] + dpNext[k]) % MOD
        }
    }

    private fun calculateS2(
        s2: Array<IntArray>,
        s1: Array<IntArray>,
        nextDp: Array<IntArray>,
        dp: Array<IntArray>,
        m2: BooleanArray,
        wl1: Int,
        wl2: Int,
    ) {
        for (j in 0..wl1) {
            calculateS2Row(s2[j], s1[j], nextDp[j], dp[j], m2, wl2)
        }
    }

    private fun calculateS2Row(
        s2Row: IntArray,
        s1Row: IntArray,
        nextDpRow: IntArray,
        dpRow: IntArray,
        m2: BooleanArray,
        wl2: Int,
    ) {
        s2Row[wl2] = 0
        for (k in wl2 - 1 downTo 0) {
            var `val` = s2Row[k + 1]
            if (m2[k]) {
                `val` = (`val` + dpRow[k + 1]) % MOD
            }
            s2Row[k] = `val`
            nextDpRow[k] = (s1Row[k] + `val`) % MOD
        }
        nextDpRow[wl2] = s1Row[wl2]
    }

    private fun helper(word1: String, target: String, tl: Int, wl1: Int): LongArray {
        var ans = LongArray(wl1 + 1)
        ans.fill(1)
        for (i in tl - 1 downTo 0) {
            ans = calculateHelperRow(word1, target[i], ans, wl1)
        }
        return ans
    }

    private fun calculateHelperRow(word: String, targetChar: Char, ans: LongArray, length: Int): LongArray {
        val next = LongArray(length + 1)
        var sum: Long = 0
        for (j in length - 1 downTo 0) {
            if (word[j] == targetChar) {
                sum = (sum + ans[j + 1]) % MOD
            }
            next[j] = sum
        }
        return next
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```