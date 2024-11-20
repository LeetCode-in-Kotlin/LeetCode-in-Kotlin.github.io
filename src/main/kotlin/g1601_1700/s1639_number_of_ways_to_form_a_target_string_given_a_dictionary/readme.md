[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1639\. Number of Ways to Form a Target String Given a Dictionary

Hard

You are given a list of strings of the **same length** `words` and a string `target`.

Your task is to form `target` using the given `words` under the following rules:

*   `target` should be formed from left to right.
*   To form the <code>i<sup>th</sup></code> character (**0-indexed**) of `target`, you can choose the <code>k<sup>th</sup></code> character of the <code>j<sup>th</sup></code> string in `words` if `target[i] = words[j][k]`.
*   Once you use the <code>k<sup>th</sup></code> character of the <code>j<sup>th</sup></code> string of `words`, you **can no longer** use the <code>x<sup>th</sup></code> character of any string in `words` where `x <= k`. In other words, all characters to the left of or at index `k` become unusuable for every string.
*   Repeat the process until you form the string `target`.

**Notice** that you can use **multiple characters** from the **same string** in `words` provided the conditions above are met.

Return _the number of ways to form `target` from `words`_. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** words = ["acca","bbbb","caca"], target = "aba"

**Output:** 6

**Explanation:** There are 6 ways to form target. 

"aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("caca") 

"aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("caca") 

"aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("acca") 

"aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("acca") 

"aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("acca") 

"aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("caca")

**Example 2:**

**Input:** words = ["abba","baab"], target = "bab"

**Output:** 4

**Explanation:** There are 4 ways to form target. 

"bab" -> index 0 ("baab"), index 1 ("baab"), index 2 ("abba") 

"bab" -> index 0 ("baab"), index 1 ("baab"), index 3 ("baab") 

"bab" -> index 0 ("baab"), index 2 ("baab"), index 3 ("baab") 

"bab" -> index 1 ("abba"), index 2 ("baab"), index 3 ("baab")

**Constraints:**

*   `1 <= words.length <= 1000`
*   `1 <= words[i].length <= 1000`
*   All strings in `words` have the same length.
*   `1 <= target.length <= 1000`
*   `words[i]` and `target` contain only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun numWays(words: Array<String>, target: String): Int {
        val counts = precompute(words)
        val memo = Array(target.length) { arrayOfNulls<Int?>(words[0].length) }
        return solve(memo, counts, words, target, 0, 0)
    }

    private fun precompute(words: Array<String>): Array<IntArray> {
        val counts = Array(words[0].length) { IntArray(26) }
        for (word in words) {
            for (idx in word.indices) {
                counts[idx][word[idx].code - 'a'.code]++
            }
        }
        return counts
    }

    private fun solve(
        memo: Array<Array<Int?>>,
        counts: Array<IntArray>,
        words: Array<String>,
        target: String,
        idx: Int,
        len: Int,
    ): Int {
        if (idx >= target.length) {
            return 1
        }
        if (len >= words[0].length || words[0].length - len < target.length - idx) {
            return 0
        }
        if (memo[idx][len] != null) {
            return memo[idx][len]!!
        }
        var answer = 0
        answer += solve(memo, counts, words, target, idx, len + 1)
        answer %= MOD
        answer += (
            solve(
                memo,
                counts,
                words,
                target,
                idx + 1,
                len + 1,
            ).toLong() * counts[len][target[idx].code - 'a'.code] %
                MOD
            ).toInt()
        answer %= MOD
        memo[idx][len] = answer
        return memo[idx][len]!!
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```