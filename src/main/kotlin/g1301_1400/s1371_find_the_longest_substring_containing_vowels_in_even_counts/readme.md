[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1371\. Find the Longest Substring Containing Vowels in Even Counts

Medium

Given the string `s`, return the size of the longest substring containing each vowel an even number of times. That is, 'a', 'e', 'i', 'o', and 'u' must appear an even number of times.

**Example 1:**

**Input:** s = "eleetminicoworoep"

**Output:** 13

**Explanation:** The longest substring is "leetminicowor" which contains two each of the vowels: **e**, **i** and **o** and zero of the vowels: **a** and **u**.

**Example 2:**

**Input:** s = "leetcodeisgreat"

**Output:** 5

**Explanation:** The longest substring is "leetc" which contains two e's.

**Example 3:**

**Input:** s = "bcbcbc"

**Output:** 6

**Explanation:** In this case, the given string "bcbcbc" is the longest because all vowels: **a**, **e**, **i**, **o** and **u** appear zero times.

**Constraints:**

*   `1 <= s.length <= 5 x 10^5`
*   `s` contains only lowercase English letters.

## Solution

```kotlin
class Solution {
    private var result: Int? = null

    fun findTheLongestSubstring(s: String): Int {
        val arr = IntArray(s.length)
        var sum = 0
        val set: Set<Char> = HashSet(mutableListOf('a', 'e', 'i', 'o', 'u'))
        for (i in 0 until s.length) {
            val c = s[i]
            if (set.contains(c)) {
                sum = if (sum and (1 shl 'a'.code - c.code) == 0) sum or (1 shl 'a'.code - c.code) else
                    sum and (1 shl 'a'.code - c.code).inv()
            }
            arr[i] = sum
        }
        for (i in 0 until s.length) {
            if (result != null && result!! > s.length - i) {
                break
            }
            for (j in s.length - 1 downTo i) {
                val e = arr[j]
                val k = if (i - 1 < 0) 0 else arr[i - 1]
                val m = e xor k
                if (m == 0) {
                    result = if (result == null) j - i + 1 else Math.max(result!!, j - i + 1)
                    break
                }
            }
        }
        return if (result == null) 0 else result!!
    }
}
```