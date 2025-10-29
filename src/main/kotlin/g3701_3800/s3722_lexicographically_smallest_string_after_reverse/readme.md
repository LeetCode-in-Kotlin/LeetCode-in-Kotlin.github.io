[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3722\. Lexicographically Smallest String After Reverse

Medium

You are given a string `s` of length `n` consisting of lowercase English letters.

You must perform **exactly** one operation by choosing any integer `k` such that `1 <= k <= n` and either:

*   reverse the **first** `k` characters of `s`, or
*   reverse the **last** `k` characters of `s`.

Return the **lexicographically smallest** string that can be obtained after **exactly** one such operation.

A string `a` is **lexicographically smaller** than a string `b` if, at the first position where they differ, `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters are the same, then the shorter string is considered lexicographically smaller.

**Example 1:**

**Input:** s = "dcab"

**Output:** "acdb"

**Explanation:**

*   Choose `k = 3`, reverse the first 3 characters.
*   Reverse `"dca"` to `"acd"`, resulting string `s = "acdb"`, which is the lexicographically smallest string achievable.

**Example 2:**

**Input:** s = "abba"

**Output:** "aabb"

**Explanation:**

*   Choose `k = 3`, reverse the last 3 characters.
*   Reverse `"bba"` to `"abb"`, so the resulting string is `"aabb"`, which is the lexicographically smallest string achievable.

**Example 3:**

**Input:** s = "zxy"

**Output:** "xzy"

**Explanation:**

*   Choose `k = 2`, reverse the first 2 characters.
*   Reverse `"zx"` to `"xz"`, so the resulting string is `"xzy"`, which is the lexicographically smallest string achievable.

**Constraints:**

*   `1 <= n == s.length <= 1000`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun lexSmallest(s: String): String {
        val n = s.length
        val arr = s.toCharArray()
        val best = arr.clone()
        // Check all reverse first k operations
        for (k in 1..n) {
            if (isBetterReverseFirstK(arr, k, best)) {
                updateBestReverseFirstK(arr, k, best)
            }
        }
        // Check all reverse last k operations
        for (k in 1..n) {
            if (isBetterReverseLastK(arr, k, best)) {
                updateBestReverseLastK(arr, k, best)
            }
        }
        return String(best)
    }

    private fun isBetterReverseFirstK(arr: CharArray, k: Int, best: CharArray): Boolean {
        for (i in arr.indices) {
            val currentChar = if (i < k) arr[k - 1 - i] else arr[i]
            if (currentChar < best[i]) {
                return true
            }
            if (currentChar > best[i]) {
                return false
            }
        }
        return false
    }

    private fun isBetterReverseLastK(arr: CharArray, k: Int, best: CharArray): Boolean {
        val n = arr.size
        for (i in 0..<n) {
            val currentChar = if (i < n - k) arr[i] else arr[n - 1 - (i - (n - k))]
            if (currentChar < best[i]) {
                return true
            }
            if (currentChar > best[i]) {
                return false
            }
        }
        return false
    }

    private fun updateBestReverseFirstK(arr: CharArray, k: Int, best: CharArray) {
        for (i in 0..<k) {
            best[i] = arr[k - 1 - i]
        }
        if (arr.size - k >= 0) {
            System.arraycopy(arr, k, best, k, arr.size - k)
        }
    }

    private fun updateBestReverseLastK(arr: CharArray, k: Int, best: CharArray) {
        val n = arr.size
        if (n - k >= 0) {
            System.arraycopy(arr, 0, best, 0, n - k)
        }
        for (i in 0..<k) {
            best[n - k + i] = arr[n - 1 - i]
        }
    }
}
```