[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2287\. Rearrange Characters to Make Target String

Easy

You are given two **0-indexed** strings `s` and `target`. You can take some letters from `s` and rearrange them to form new strings.

Return _the **maximum** number of copies of_ `target` _that can be formed by taking letters from_ `s` _and rearranging them._

**Example 1:**

**Input:** s = "ilovecodingonleetcode", target = "code"

**Output:** 2

**Explanation:**

For the first copy of "code", take the letters at indices 4, 5, 6, and 7.

For the second copy of "code", take the letters at indices 17, 18, 19, and 20.

The strings that are formed are "ecod" and "code" which can both be rearranged into "code".

We can make at most two copies of "code", so we return 2. 

**Example 2:**

**Input:** s = "abcba", target = "abc"

**Output:** 1

**Explanation:**

We can make one copy of "abc" by taking the letters at indices 0, 1, and 2.

We can make at most one copy of "abc", so we return 1.

Note that while there is an extra 'a' and 'b' at indices 3 and 4, we cannot reuse the letter 'c' at index 2, so we cannot make a second copy of "abc". 

**Example 3:**

**Input:** s = "abbaccaddaeea", target = "aaaaa"

**Output:** 1

**Explanation:**

We can make one copy of "aaaaa" by taking the letters at indices 0, 3, 6, 9, and 12.

We can make at most one copy of "aaaaa", so we return 1. 

**Constraints:**

*   `1 <= s.length <= 100`
*   `1 <= target.length <= 10`
*   `s` and `target` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun rearrangeCharacters(s: String, target: String): Int {
        return getMaxCopies(target, getCharCount(s), getCharCount(target))
    }

    private fun getCharCount(str: String): IntArray {
        val charToCount = IntArray(26)
        for (i in 0 until str.length) {
            charToCount[str[i].code - 'a'.code]++
        }
        return charToCount
    }

    private fun getMaxCopies(target: String, sCount: IntArray, tCount: IntArray): Int {
        var copies = Int.MAX_VALUE
        for (i in 0 until target.length) {
            val ch = target[i].code - 'a'.code
            copies = Math.min(copies, sCount[ch] / tCount[ch])
        }
        return copies
    }
}
```