[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3563\. Lexicographically Smallest String After Adjacent Removals

Hard

You are given a string `s` consisting of lowercase English letters.

You can perform the following operation any number of times (including zero):

*   Remove **any** pair of **adjacent** characters in the string that are **consecutive** in the alphabet, in either order (e.g., `'a'` and `'b'`, or `'b'` and `'a'`).
*   Shift the remaining characters to the left to fill the gap.

Return the **lexicographically smallest** string that can be obtained after performing the operations optimally.

**Note:** Consider the alphabet as circular, thus `'a'` and `'z'` are consecutive.

**Example 1:**

**Input:** s = "abc"

**Output:** "a"

**Explanation:**

*   Remove `"bc"` from the string, leaving `"a"` as the remaining string.
*   No further operations are possible. Thus, the lexicographically smallest string after all possible removals is `"a"`.

**Example 2:**

**Input:** s = "bcda"

**Output:** ""

**Explanation:**

*   Remove `"cd"` from the string, leaving `"ba"` as the remaining string.
*   Remove `"ba"` from the string, leaving `""` as the remaining string.
*   No further operations are possible. Thus, the lexicographically smallest string after all possible removals is `""`.

**Example 3:**

**Input:** s = "zdce"

**Output:** "zdce"

**Explanation:**

*   Remove `"dc"` from the string, leaving `"ze"` as the remaining string.
*   No further operations are possible on `"ze"`.
*   However, since `"zdce"` is lexicographically smaller than `"ze"`, the smallest string after all possible removals is `"zdce"`.

**Constraints:**

*   `1 <= s.length <= 250`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    private fun checkPair(char1: Char, char2: Char): Boolean {
        val diffVal = abs(char1.code - char2.code)
        return diffVal == 1 || (char1 == 'a' && char2 == 'z') || (char1 == 'z' && char2 == 'a')
    }

    fun lexicographicallySmallestString(sIn: String): String? {
        val nVal = sIn.length
        if (nVal == 0) {
            return ""
        }
        val remTable = Array<BooleanArray?>(nVal) { BooleanArray(nVal) }
        var len = 2
        while (len <= nVal) {
            for (idx in 0..nVal - len) {
                val j = idx + len - 1
                if (checkPair(sIn[idx], sIn[j])) {
                    if (len == 2) {
                        remTable[idx]!![j] = true
                    } else {
                        if (remTable[idx + 1]!![j - 1]) {
                            remTable[idx]!![j] = true
                        }
                    }
                }
                if (remTable[idx]!![j]) {
                    continue
                }
                var pSplit = idx + 1
                while (pSplit < j) {
                    if (remTable[idx]!![pSplit] && remTable[pSplit + 1]!![j]) {
                        remTable[idx]!![j] = true
                        break
                    }
                    pSplit += 2
                }
            }
            len += 2
        }
        val dpArr: Array<String> = Array<String>(nVal + 1) { "" }
        dpArr[nVal] = ""
        for (idx in nVal - 1 downTo 0) {
            dpArr[idx] = sIn[idx].toString() + dpArr[idx + 1]
            for (kMatch in idx + 1..<nVal) {
                if (checkPair(sIn[idx], sIn[kMatch])) {
                    val middleVanishes: Boolean = if (kMatch - 1 < idx + 1) {
                        true
                    } else {
                        remTable[idx + 1]!![kMatch - 1]
                    }
                    if (middleVanishes) {
                        val candidate = dpArr[kMatch + 1]
                        if (candidate < dpArr[idx]) {
                            dpArr[idx] = candidate
                        }
                    }
                }
            }
        }
        return dpArr[0]
    }
}
```