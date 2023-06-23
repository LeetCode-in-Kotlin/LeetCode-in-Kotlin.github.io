[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2027\. Minimum Moves to Convert String

Easy

You are given a string `s` consisting of `n` characters which are either `'X'` or `'O'`.

A **move** is defined as selecting **three** **consecutive characters** of `s` and converting them to `'O'`. Note that if a move is applied to the character `'O'`, it will stay the **same**.

Return _the **minimum** number of moves required so that all the characters of_ `s` _are converted to_ `'O'`.

**Example 1:**

**Input:** s = "XXX"

**Output:** 1

**Explanation:** XXX -> OOO We select all the 3 characters and convert them in one move.

**Example 2:**

**Input:** s = "XXOX"

**Output:** 2

**Explanation:** XXOX -> OOOX -> OOOO 

We select the first 3 characters in the first move, and convert them to `'O'`. 

Then we select the last 3 characters and convert them so that the final string contains all `'O'`s.

**Example 3:**

**Input:** s = "OOOO"

**Output:** 0

**Explanation:** There are no `'X's` in `s` to convert.

**Constraints:**

*   `3 <= s.length <= 1000`
*   `s[i]` is either `'X'` or `'O'`.

## Solution

```kotlin
class Solution {
    fun minimumMoves(s: String): Int {
        var r = 0
        var i = 0
        val sArray = s.toCharArray()
        while (i < sArray.size) {
            if (sArray[i] == 'X') {
                r++
                i += 2
            }
            i++
        }
        return r
    }
}
```