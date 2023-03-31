[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 854\. K-Similar Strings

Hard

Strings `s1` and `s2` are `k`**\-similar** (for some non-negative integer `k`) if we can swap the positions of two letters in `s1` exactly `k` times so that the resulting string equals `s2`.

Given two anagrams `s1` and `s2`, return the smallest `k` for which `s1` and `s2` are `k`**\-similar**.

**Example 1:**

**Input:** s1 = "ab", s2 = "ba"

**Output:** 1

**Explanation:** The two string are 1-similar because we can use one swap to change s1 to s2: "ab" --> "ba".

**Example 2:**

**Input:** s1 = "abc", s2 = "bca"

**Output:** 2

**Explanation:** The two strings are 2-similar because we can use two swaps to change s1 to s2: "abc" --> "bac" --> "bca".

**Constraints:**

*   `1 <= s1.length <= 20`
*   `s2.length == s1.length`
*   `s1` and `s2` contain only lowercase letters from the set `{'a', 'b', 'c', 'd', 'e', 'f'}`.
*   `s2` is an anagram of `s1`.

## Solution

```kotlin
class Solution {
    fun kSimilarity(a: String, b: String): Int {
        var ans = 0
        val achars = a.toCharArray()
        val bchars = b.toCharArray()
        ans += getAllPerfectMatches(achars, bchars)
        for (i in achars.indices) {
            if (achars[i] == bchars[i]) {
                continue
            }
            return ans + checkAllOptions(achars, bchars, i, b)
        }
        return ans
    }

    private fun checkAllOptions(achars: CharArray, bchars: CharArray, i: Int, b: String): Int {
        var ans = Int.MAX_VALUE
        for (j in i + 1 until achars.size) {
            if (achars[j] == bchars[i] && achars[j] != bchars[j]) {
                swap(achars, i, j)
                ans = Math.min(ans, 1 + kSimilarity(String(achars), b))
                swap(achars, i, j)
            }
        }
        return ans
    }

    private fun getAllPerfectMatches(achars: CharArray, bchars: CharArray): Int {
        var ans = 0
        for (i in achars.indices) {
            if (achars[i] == bchars[i]) {
                continue
            }
            for (j in i + 1 until achars.size) {
                if (achars[j] == bchars[i] && bchars[j] == achars[i]) {
                    swap(achars, i, j)
                    ans++
                    break
                }
            }
        }
        return ans
    }

    private fun swap(a: CharArray, i: Int, j: Int) {
        val temp = a[i]
        a[i] = a[j]
        a[j] = temp
    }
}
```