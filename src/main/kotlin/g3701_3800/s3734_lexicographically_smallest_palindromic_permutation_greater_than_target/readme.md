[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3734\. Lexicographically Smallest Palindromic Permutation Greater Than Target

Hard

You are given two strings `s` and `target`, each of length `n`, consisting of lowercase English letters.

Return the **lexicographically smallest string** that is **both** a **palindromic permutation** of `s` and **strictly** greater than `target`. If no such permutation exists, return an empty string.

**Example 1:**

**Input:** s = "baba", target = "abba"

**Output:** "baab"

**Explanation:**

*   The palindromic permutations of `s` (in lexicographical order) are `"abba"` and `"baab"`.
*   The lexicographically smallest permutation that is strictly greater than `target` is `"baab"`.

**Example 2:**

**Input:** s = "baba", target = "bbaa"

**Output:** ""

**Explanation:**

*   The palindromic permutations of `s` (in lexicographical order) are `"abba"` and `"baab"`.
*   None of them is lexicographically strictly greater than `target`. Therefore, the answer is `""`.

**Example 3:**

**Input:** s = "abc", target = "abb"

**Output:** ""

**Explanation:**

`s` has no palindromic permutations. Therefore, the answer is `""`.

**Example 4:**

**Input:** s = "aac", target = "abb"

**Output:** "aca"

**Explanation:**

*   The only palindromic permutation of `s` is `"aca"`.
*   `"aca"` is strictly greater than `target`. Therefore, the answer is `"aca"`.

**Constraints:**

*   `1 <= n == s.length == target.length <= 300`
*   `s` and `target` consist of only lowercase English letters.

## Solution

```kotlin
class Solution {
    internal fun func(i: Int, target: String, ans: CharArray, l: Int, r: Int, freq: IntArray, end: Boolean): Boolean {
        if (l > r) {
            return String(ans).compareTo(target) > 0
        }
        if (l == r) {
            var left = '#'
            for (k in 0 until 26) {
                if (freq[k] > 0) {
                    left = (k + 'a'.code).toChar()
                }
            }
            freq[left.code - 'a'.code]--
            ans[l] = left
            if (func(i + 1, target, ans, l + 1, r - 1, freq, end)) {
                return true
            }
            freq[left.code - 'a'.code]++
            ans[l] = '#'
            return false
        }
        if (end) {
            for (j in 0 until 26) {
                if (freq[j] > 1) {
                    freq[j] -= 2
                    val charJ = (j + 'a'.code).toChar()
                    ans[l] = charJ
                    ans[r] = charJ
                    if (func(i + 1, target, ans, l + 1, r - 1, freq, end)) {
                        return true
                    }
                    ans[l] = '#'
                    ans[r] = '#'
                    freq[j] += 2
                }
            }
            return false
        }
        val curr = target[i]
        var next = '1'
        for (k in (curr.code - 'a'.code + 1) until 26) {
            if (freq[k] > 1) {
                next = (k + 'a'.code).toChar()
                break
            }
        }
        if (freq[curr.code - 'a'.code] > 1) {
            ans[l] = curr
            ans[r] = curr
            freq[curr.code - 'a'.code] -= 2
            if (func(i + 1, target, ans, l + 1, r - 1, freq, false)) { // end = false
                return true
            }
            freq[curr.code - 'a'.code] += 2
        }
        if (next != '1') {
            ans[l] = next
            ans[r] = next
            freq[next.code - 'a'.code] -= 2
            if (func(i + 1, target, ans, l + 1, r - 1, freq, true)) { // end = true
                return true
            }
        }
        ans[l] = '#'
        ans[r] = '#'
        return false
    }

    fun lexPalindromicPermutation(s: String, target: String): String {
        val freq = IntArray(26)
        for (char in s) {
            freq[char.code - 'a'.code]++
        }
        var oddc = 0
        for (i in 0 until 26) {
            if (freq[i] % 2 == 1) {
                oddc++
            }
        }
        if (oddc > 1) {
            return ""
        }
        val ans = CharArray(s.length) { '#' }
        if (func(0, target, ans, 0, s.length - 1, freq, false)) {
            return String(ans)
        }
        return ""
    }
}
```