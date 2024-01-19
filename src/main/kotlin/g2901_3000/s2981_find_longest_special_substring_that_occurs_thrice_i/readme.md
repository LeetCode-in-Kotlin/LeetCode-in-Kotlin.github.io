[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2981\. Find Longest Special Substring That Occurs Thrice I

Medium

You are given a string `s` that consists of lowercase English letters.

A string is called **special** if it is made up of only a single character. For example, the string `"abc"` is not special, whereas the strings `"ddd"`, `"zz"`, and `"f"` are special.

Return _the length of the **longest special substring** of_ `s` _which occurs **at least thrice**_, _or_ `-1` _if no special substring occurs at least thrice_.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "aaaa"

**Output:** 2

**Explanation:** The longest special substring which occurs thrice is "aa": substrings "<ins>**aa**</ins>aa", "a<ins>**aa**</ins>a", and "aa<ins>**aa**</ins>".

It can be shown that the maximum length achievable is 2. 

**Example 2:**

**Input:** s = "abcdef"

**Output:** -1

**Explanation:** There exists no special substring which occurs at least thrice. Hence return -1. 

**Example 3:**

**Input:** s = "abcaba"

**Output:** 1

**Explanation:** The longest special substring which occurs thrice is "a": substrings "<ins>**a**</ins>bcaba", "abc<ins>**a**</ins>ba", and "abcab<ins>**a**</ins>".

It can be shown that the maximum length achievable is 1. 

**Constraints:**

*   `3 <= s.length <= 50`
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
import java.util.Collections
import java.util.TreeMap
import kotlin.math.max

class Solution {
    fun maximumLength(s: String): Int {
        val buckets: MutableList<MutableList<Int>> = ArrayList()
        for (i in 0..25) {
            buckets.add(ArrayList())
        }
        var cur = 1
        for (i in 1 until s.length) {
            if (s[i] != s[i - 1]) {
                val index = s[i - 1].code - 'a'.code
                buckets[index].add(cur)
                cur = 1
            } else {
                cur++
            }
        }
        val endIndex = s[s.length - 1].code - 'a'.code
        buckets[endIndex].add(cur)
        var result = -1
        for (bucket in buckets) {
            result = max(result, generate(bucket))
        }
        return result
    }

    private fun generate(list: List<Int>): Int {
        Collections.sort(list, Collections.reverseOrder())
        val map = TreeMap<Int, Int>(Collections.reverseOrder())
        var i = 0
        while (i < list.size && i < 3) {
            val cur = list[i]
            var num = map.getOrDefault(cur, 0)
            map[cur] = num + 1
            if (cur >= 2) {
                num = map.getOrDefault(cur - 1, 0)
                map[cur - 1] = num + 2
            }
            if (cur >= 3) {
                num = map.getOrDefault(cur - 2, 0)
                map[cur - 2] = num + 3
            }
            i++
        }
        for ((key, value) in map) {
            if (value >= 3) {
                return key
            }
        }
        return -1
    }
}
```