[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3545\. Minimum Deletions for At Most K Distinct Characters

Easy

You are given a string `s` consisting of lowercase English letters, and an integer `k`.

Your task is to delete some (possibly none) of the characters in the string so that the number of **distinct** characters in the resulting string is **at most** `k`.

Return the **minimum** number of deletions required to achieve this.

**Example 1:**

**Input:** s = "abc", k = 2

**Output:** 1

**Explanation:**

*   `s` has three distinct characters: `'a'`, `'b'` and `'c'`, each with a frequency of 1.
*   Since we can have at most `k = 2` distinct characters, remove all occurrences of any one character from the string.
*   For example, removing all occurrences of `'c'` results in at most `k` distinct characters. Thus, the answer is 1.

**Example 2:**

**Input:** s = "aabb", k = 2

**Output:** 0

**Explanation:**

*   `s` has two distinct characters (`'a'` and `'b'`) with frequencies of 2 and 2, respectively.
*   Since we can have at most `k = 2` distinct characters, no deletions are required. Thus, the answer is 0.

**Example 3:**

**Input:** s = "yyyzz", k = 1

**Output:** 2

**Explanation:**

*   `s` has two distinct characters (`'y'` and `'z'`) with frequencies of 3 and 2, respectively.
*   Since we can have at most `k = 1` distinct character, remove all occurrences of any one character from the string.
*   Removing all `'z'` results in at most `k` distinct characters. Thus, the answer is 2.

**Constraints:**

*   `1 <= s.length <= 16`
*   `1 <= k <= 16`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minDeletion(s: String, k: Int): Int {
        val n = s.length
        var count = 0
        val carr = IntArray(26)
        for (i in 0..<n) {
            val ch = s[i]
            carr[ch.code - 'a'.code]++
        }
        var dischar = 0
        for (i in 0..25) {
            if (carr[i] > 0) {
                dischar++
            }
        }
        while (dischar > k) {
            var minF = Int.Companion.MAX_VALUE
            var idx = -1
            for (i in 0..25) {
                if ((carr[i] > 0) && minF > carr[i]) {
                    minF = carr[i]
                    idx = i
                }
            }
            count += minF
            carr[idx] = 0
            dischar--
        }
        return count
    }
}
```