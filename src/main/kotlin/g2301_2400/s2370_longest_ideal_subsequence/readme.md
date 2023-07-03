[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2370\. Longest Ideal Subsequence

Medium

You are given a string `s` consisting of lowercase letters and an integer `k`. We call a string `t` **ideal** if the following conditions are satisfied:

*   `t` is a **subsequence** of the string `s`.
*   The absolute difference in the alphabet order of every two **adjacent** letters in `t` is less than or equal to `k`.

Return _the length of the **longest** ideal string_.

A **subsequence** is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

**Note** that the alphabet order is not cyclic. For example, the absolute difference in the alphabet order of `'a'` and `'z'` is `25`, not `1`.

**Example 1:**

**Input:** s = "acfgbd", k = 2

**Output:** 4

**Explanation:** The longest ideal string is "acbd". The length of this string is 4, so 4 is returned. Note that "acfgbd" is not ideal because 'c' and 'f' have a difference of 3 in alphabet order.

**Example 2:**

**Input:** s = "abcd", k = 3

**Output:** 4

**Explanation:** The longest ideal string is "abcd". The length of this string is 4, so 4 is returned.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `0 <= k <= 25`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun longestIdealString(s: String, k: Int): Int {
        var ans = 1
        val array = IntArray(26)
        for (i in 0 until s.length) {
            val curr = s[i].code - 'a'.code
            var currans = 1
            var temp = k
            array[curr] += 1
            var j = curr - 1
            while (temp > 0) {
                if (j == -1) {
                    break
                }
                currans = Math.max(currans, array[j] + 1)
                temp--
                if (j == 0) {
                    break
                }
                j--
            }
            temp = k
            j = curr + 1
            while (temp > 0) {
                if (j == 26) {
                    break
                }
                currans = Math.max(currans, array[j] + 1)
                temp--
                if (j == 25) {
                    break
                }
                j++
            }
            array[curr] = Math.max(currans, array[curr])
            ans = Math.max(ans, array[curr])
        }
        return ans
    }
}
```