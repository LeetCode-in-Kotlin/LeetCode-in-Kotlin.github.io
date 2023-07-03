[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2002\. Maximum Product of the Length of Two Palindromic Subsequences

Medium

Given a string `s`, find two **disjoint palindromic subsequences** of `s` such that the **product** of their lengths is **maximized**. The two subsequences are **disjoint** if they do not both pick a character at the same index.

Return _the **maximum** possible **product** of the lengths of the two palindromic subsequences_.

A **subsequence** is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters. A string is **palindromic** if it reads the same forward and backward.

**Example 1:**

![example-1](https://assets.leetcode.com/uploads/2021/08/24/two-palindromic-subsequences.png)

**Input:** s = "leetcodecom"

**Output:** 9

**Explanation:** An optimal solution is to choose "ete" for the 1<sup>st</sup> subsequence and "cdc" for the 2<sup>nd</sup> subsequence.

The product of their lengths is: 3 \* 3 = 9. 

**Example 2:**

**Input:** s = "bb"

**Output:** 1

**Explanation:** An optimal solution is to choose "b" (the first character) for the 1<sup>st</sup> subsequence and "b" (the second character) for the 2<sup>nd</sup> subsequence.

The product of their lengths is: 1 \* 1 = 1. 

**Example 3:**

**Input:** s = "accbcaxxcxx"

**Output:** 25

**Explanation:** An optimal solution is to choose "accca" for the 1<sup>st</sup> subsequence and "xxcxx" for the 2<sup>nd</sup> subsequence.

The product of their lengths is: 5 \* 5 = 25. 

**Constraints:**

*   `2 <= s.length <= 12`
*   `s` consists of lowercase English letters only.

## Solution

```kotlin
class Solution {
    fun maxProduct(s: String): Int {
        if (s.length == 2) {
            return 1
        }
        val list: MutableList<State> = ArrayList()
        val chars = s.toCharArray()
        val visited: MutableSet<State> = HashSet()
        for (i in chars.indices) {
            val mask = 1 shl i
            recur(chars, State(i, i, 0, mask), list, visited)
            recur(chars, State(i, i + 1, 0, mask), list, visited)
        }
        list.sortWith { a: State, b: State -> b.cnt - a.cnt }
        var res = 1
        val explored: MutableSet<Int> = HashSet()
        for (i in 0 until list.size - 1) {
            if (explored.contains(i)) {
                continue
            }
            val cur = list[i]
            if (cur.cnt == 1) {
                break
            }
            for (j in i + 1 until list.size) {
                val cand = list[j]
                if (cur.mask and cand.mask < 1) {
                    if (explored.add(j)) {
                        res = res.coerceAtLeast(cur.cnt * cand.cnt)
                    }
                    break
                }
            }
        }
        return res
    }

    private fun recur(chars: CharArray, s: State, list: MutableList<State>, visited: MutableSet<State>) {
        if (s.i < 0 || s.j >= chars.size) {
            return
        }
        if (!visited.add(s)) {
            return
        }
        if (chars[s.i] == chars[s.j]) {
            val m = s.mask or (1 shl s.i) or (1 shl s.j)
            val nextCnt = s.cnt + if (s.i < s.j) 2 else 1
            list.add(State(s.i, s.j, nextCnt, m))
            recur(chars, State(s.i - 1, s.j + 1, nextCnt, m), list, visited)
        }
        recur(chars, State(s.i - 1, s.j, s.cnt, s.mask), list, visited)
        recur(chars, State(s.i, s.j + 1, s.cnt, s.mask), list, visited)
    }

    private class State(var i: Int, var j: Int, var cnt: Int, var mask: Int) {
        override fun equals(other: Any?): Boolean {
            if (other == null || other.javaClass != this.javaClass) {
                return false
            }
            val s = other as State
            return i == s.i && j == s.j && mask == s.mask
        }

        override fun hashCode(): Int {
            return (i * 31 + j) * 31 + mask
        }
    }
}
```