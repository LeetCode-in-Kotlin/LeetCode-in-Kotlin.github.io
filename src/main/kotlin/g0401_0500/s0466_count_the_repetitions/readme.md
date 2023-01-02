[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 466\. Count The Repetitions

Hard

We define `str = [s, n]` as the string `str` which consists of the string `s` concatenated `n` times.

*   For example, `str == ["abc", 3] =="abcabcabc"`.

We define that string `s1` can be obtained from string `s2` if we can remove some characters from `s2` such that it becomes `s1`.

*   For example, `s1 = "abc"` can be obtained from <code>s2 = "ab**<ins>dbe</ins>**c"</code> based on our definition by removing the bolded underlined characters.

You are given two strings `s1` and `s2` and two integers `n1` and `n2`. You have the two strings `str1 = [s1, n1]` and `str2 = [s2, n2]`.

Return _the maximum integer_ `m` _such that_ `str = [str2, m]` _can be obtained from_ `str1`.

**Example 1:**

**Input:** s1 = "acb", n1 = 4, s2 = "ab", n2 = 2

**Output:** 2

**Example 2:**

**Input:** s1 = "acb", n1 = 1, s2 = "acb", n2 = 1

**Output:** 1

**Constraints:**

*   `1 <= s1.length, s2.length <= 100`
*   `s1` and `s2` consist of lowercase English letters.
*   <code>1 <= n1, n2 <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun getMaxRepetitions(s1: String, n1: Int, s2: String, n2: Int): Int {
        val n = s2.length
        val ss1 = s1.toCharArray()
        val ss2 = s2.toCharArray()
        val memo = arrayOfNulls<IntArray>(n)
        val s2CountMap = IntArray(n + 1)
        var s1Count = 0
        var s2Count = 0
        var s2Idx = 0
        while (memo[s2Idx] == null) {
            memo[s2Idx] = intArrayOf(s1Count, s2Count)
            for (c1 in ss1) {
                if (c1 == ss2[s2Idx]) {
                    s2Idx++
                    if (s2Idx == n) {
                        s2Count++
                        s2Idx = 0
                    }
                }
            }
            s1Count++
            s2CountMap[s1Count] = s2Count
        }
        var n1Left = n1 - memo[s2Idx]!![0]
        val matchedPatternCount = n1Left / (s1Count - memo[s2Idx]!![0]) * (s2Count - memo[s2Idx]!![1])
        n1Left = n1Left % (s1Count - memo[s2Idx]!![0])
        val leftS2Count = s2CountMap[memo[s2Idx]!![0] + n1Left] - memo[s2Idx]!![1]
        val totalCount = leftS2Count + matchedPatternCount + memo[s2Idx]!![1]
        return totalCount / n2
    }
}
```