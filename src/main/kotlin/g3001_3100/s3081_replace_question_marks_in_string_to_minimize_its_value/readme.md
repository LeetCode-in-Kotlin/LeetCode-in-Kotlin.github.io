[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3081\. Replace Question Marks in String to Minimize Its Value

Medium

You are given a string `s`. `s[i]` is either a lowercase English letter or `'?'`.

For a string `t` having length `m` containing **only** lowercase English letters, we define the function `cost(i)` for an index `i` as the number of characters **equal** to `t[i]` that appeared before it, i.e. in the range `[0, i - 1]`.

The **value** of `t` is the **sum** of `cost(i)` for all indices `i`.

For example, for the string `t = "aab"`:

*   `cost(0) = 0`
*   `cost(1) = 1`
*   `cost(2) = 0`
*   Hence, the value of `"aab"` is `0 + 1 + 0 = 1`.

Your task is to **replace all** occurrences of `'?'` in `s` with any lowercase English letter so that the **value** of `s` is **minimized**.

Return _a string denoting the modified string with replaced occurrences of_ `'?'`_. If there are multiple strings resulting in the **minimum value**, return the lexicographically smallest one._

**Example 1:**

**Input:** s = "???"

**Output:** "abc"

**Explanation:** In this example, we can replace the occurrences of `'?'` to make `s` equal to `"abc"`.

For `"abc"`, `cost(0) = 0`, `cost(1) = 0`, and `cost(2) = 0`.

The value of `"abc"` is `0`.

Some other modifications of `s` that have a value of `0` are `"cba"`, `"abz"`, and, `"hey"`.

Among all of them, we choose the lexicographically smallest.

**Example 2:**

**Input:** s = "a?a?"

**Output:** "abac"

**Explanation:** In this example, the occurrences of `'?'` can be replaced to make `s` equal to `"abac"`.

For `"abac"`, `cost(0) = 0`, `cost(1) = 0`, `cost(2) = 1`, and `cost(3) = 0`.

The value of `"abac"` is `1`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either a lowercase English letter or `'?'`.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimizeStringValue(s: String): String {
        val n = s.length
        var time = 0
        val count = IntArray(26)
        val res = IntArray(26)
        val str = s.toCharArray()
        for (c in str) {
            if (c != '?') {
                count[c.code - 'a'.code]++
            } else {
                time++
            }
        }
        var minTime = Int.MAX_VALUE
        for (i in 0..25) {
            minTime = min(minTime, count[i])
        }
        while (time > 0) {
            for (j in 0..25) {
                if (count[j] == minTime) {
                    res[j]++
                    count[j]++
                    time--
                }
                if (time == 0) {
                    break
                }
            }
            minTime++
        }
        var start = 0
        for (i in 0 until n) {
            if (str[i] == '?') {
                while (res[start] == 0) {
                    start++
                }
                str[i] = ('a'.code + start).toChar()
                res[start]--
            }
        }
        return String(str)
    }
}
```