[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2707\. Extra Characters in a String

Medium

You are given a **0-indexed** string `s` and a dictionary of words `dictionary`. You have to break `s` into one or more **non-overlapping** substrings such that each substring is present in `dictionary`. There may be some **extra characters** in `s` which are not present in any of the substrings.

Return _the **minimum** number of extra characters left over if you break up_ `s` _optimally._

**Example 1:**

**Input:** s = "leetscode", dictionary = ["leet","code","leetcode"]

**Output:** 1

**Explanation:** We can break s in two substrings: "leet" from index 0 to 3 and "code" from index 5 to 8. There is only 1 unused character (at index 4), so we return 1.

**Example 2:**

**Input:** s = "sayhelloworld", dictionary = ["hello","world"]

**Output:** 3

**Explanation:** We can break s in two substrings: "hello" from index 3 to 7 and "world" from index 8 to 12. The characters at indices 0, 1, 2 are not used in any substring and thus are considered as extra characters. Hence, we return 3.

**Constraints:**

*   `1 <= s.length <= 50`
*   `1 <= dictionary.length <= 50`
*   `1 <= dictionary[i].length <= 50`
*   `dictionary[i]` and `s` consists of only lowercase English letters
*   `dictionary` contains distinct words

## Solution

```kotlin
class Solution {
    fun minExtraChar(s: String, dictionary: Array<String>): Int {
        val dict: MutableSet<String> = HashSet()
        val dp = IntArray(s.length + 1)
        for (word in dictionary) dict.add(word)
        for (i in 1 until dp.size) {
            dp[i] = dp[i - 1] + 1
            for (j in i - 1 downTo 0) {
                val sub: String = s.substring(j, i)
                if (dict.contains(sub)) dp[i] = dp[i].coerceAtMost(dp[j])
            }
        }
        return dp[dp.size - 1]
    }
}
```