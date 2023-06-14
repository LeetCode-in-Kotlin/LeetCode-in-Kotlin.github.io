[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1528\. Shuffle String

Easy

You are given a string `s` and an integer array `indices` of the **same length**. The string `s` will be shuffled such that the character at the <code>i<sup>th</sup></code> position moves to `indices[i]` in the shuffled string.

Return _the shuffled string_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/09/q1.jpg)

**Input:** s = "codeleet", `indices` = [4,5,6,7,0,2,1,3]

**Output:** "leetcode"

**Explanation:** As shown, "codeleet" becomes "leetcode" after shuffling.

**Example 2:**

**Input:** s = "abc", `indices` = [0,1,2]

**Output:** "abc"

**Explanation:** After shuffling, each character remains in its position.

**Constraints:**

*   `s.length == indices.length == n`
*   `1 <= n <= 100`
*   `s` consists of only lowercase English letters.
*   `0 <= indices[i] < n`
*   All values of `indices` are **unique**.

## Solution

```kotlin
class Solution {
    fun restoreString(s: String, indices: IntArray): String {
        val c = CharArray(s.length)
        for (i in 0 until s.length) {
            val index = findIndex(indices, i)
            c[i] = s[index]
        }
        return String(c)
    }

    companion object {
        private fun findIndex(indices: IntArray, i: Int): Int {
            for (j in indices.indices) {
                if (indices[j] == i) {
                    return j
                }
            }
            return 0
        }
    }
}
```