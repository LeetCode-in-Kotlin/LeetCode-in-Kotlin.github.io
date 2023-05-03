[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 936\. Stamping The Sequence

Hard

You are given two strings `stamp` and `target`. Initially, there is a string `s` of length `target.length` with all `s[i] == '?'`.

In one turn, you can place `stamp` over `s` and replace every letter in the `s` with the corresponding letter from `stamp`.

*   For example, if `stamp = "abc"` and `target = "abcba"`, then `s` is `"?????"` initially. In one turn you can:

    *   place `stamp` at index `0` of `s` to obtain `"abc??"`,
    *   place `stamp` at index `1` of `s` to obtain `"?abc?"`, or
    *   place `stamp` at index `2` of `s` to obtain `"??abc"`.

    Note that `stamp` must be fully contained in the boundaries of `s` in order to stamp (i.e., you cannot place `stamp` at index `3` of `s`).

We want to convert `s` to `target` using **at most** `10 * target.length` turns.

Return _an array of the index of the left-most letter being stamped at each turn_. If we cannot obtain `target` from `s` within `10 * target.length` turns, return an empty array.

**Example 1:**

**Input:** stamp = "abc", target = "ababc"

**Output:** [0,2]

**Explanation:** Initially s = "?????". 

- Place stamp at index 0 to get "abc??". 

- Place stamp at index 2 to get "ababc".

[1,0,2] would also be accepted as an answer, as well as some other answers.

**Example 2:**

**Input:** stamp = "abca", target = "aabcaca"

**Output:** [3,0,1]

**Explanation:** Initially s = "???????".

- Place stamp at index 3 to get "???abca". 

- Place stamp at index 0 to get "abcabca". 

- Place stamp at index 1 to get "aabcaca".

**Constraints:**

*   `1 <= stamp.length <= target.length <= 1000`
*   `stamp` and `target` consist of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun movesToStamp(stamp: String, target: String): IntArray {
        val moves: MutableList<Int> = ArrayList()
        val s = stamp.toCharArray()
        val t = target.toCharArray()
        var stars = 0
        val visited = BooleanArray(target.length)
        while (stars < target.length) {
            var doneReplace = false
            for (i in 0..target.length - stamp.length) {
                if (!visited[i] && canReplace(t, i, s)) {
                    stars = doReplace(t, i, s, stars)
                    doneReplace = true
                    visited[i] = true
                    moves.add(i)
                    if (stars == t.size) {
                        break
                    }
                }
            }
            if (!doneReplace) {
                return IntArray(0)
            }
        }
        val result = IntArray(moves.size)
        for (i in moves.indices) {
            result[i] = moves[moves.size - i - 1]
        }
        return result
    }

    private fun canReplace(t: CharArray, i: Int, s: CharArray): Boolean {
        for (j in s.indices) {
            if (t[i + j] != '*' && t[i + j] != s[j]) {
                return false
            }
        }
        return true
    }

    private fun doReplace(t: CharArray, i: Int, s: CharArray, stars: Int): Int {
        var stars = stars
        for (j in s.indices) {
            if (t[i + j] != '*') {
                t[i + j] = '*'
                stars++
            }
        }
        return stars
    }
}
```