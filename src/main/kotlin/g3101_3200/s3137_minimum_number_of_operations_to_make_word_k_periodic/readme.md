[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3137\. Minimum Number of Operations to Make Word K-Periodic

Medium

You are given a string `word` of size `n`, and an integer `k` such that `k` divides `n`.

In one operation, you can pick any two indices `i` and `j`, that are divisible by `k`, then replace the substring of length `k` starting at `i` with the substring of length `k` starting at `j`. That is, replace the substring `word[i..i + k - 1]` with the substring `word[j..j + k - 1]`.

Return _the **minimum** number of operations required to make_ `word` _**k-periodic**_.

We say that `word` is **k-periodic** if there is some string `s` of length `k` such that `word` can be obtained by concatenating `s` an arbitrary number of times. For example, if `word == “ababab”`, then `word` is 2-periodic for `s = "ab"`.

**Example 1:**

**Input:** word = "leetcodeleet", k = 4

**Output:** 1

**Explanation:**

We can obtain a 4-periodic string by picking i = 4 and j = 0. After this operation, word becomes equal to "leetleetleet".

**Example 2:**

**Input:** word = "leetcoleet", k = 2

**Output:** 3

**Explanation:**

We can obtain a 2-periodic string by applying the operations in the table below.

    i  j  word
    0  2  etetcoleet
    4  0  etetetleet
    6  0  etetetetet

**Constraints:**

*   <code>1 <= n == word.length <= 10<sup>5</sup></code>
*   `1 <= k <= word.length`
*   `k` divides `word.length`.
*   `word` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun minimumOperationsToMakeKPeriodic(word: String, k: Int): Int {
        val map: MutableMap<Int, Int> = HashMap()
        val n = word.length
        var max = 0
        var i = 0
        while (i < n) {
            var hash = 0
            for (j in i until i + k) {
                val idx = word[j].code - 'a'.code
                hash = hash * 26 + idx
            }
            var count = map.getOrDefault(hash, 0)
            count++
            map[hash] = count
            max = max(max, count)
            i += k
        }
        return n / k - max
    }
}
```