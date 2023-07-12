[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2573\. Find the String with LCP

Hard

We define the `lcp` matrix of any **0-indexed** string `word` of `n` lowercase English letters as an `n x n` grid such that:

*   `lcp[i][j]` is equal to the length of the **longest common prefix** between the substrings `word[i,n-1]` and `word[j,n-1]`.

Given an `n x n` matrix `lcp`, return the alphabetically smallest string `word` that corresponds to `lcp`. If there is no such string, return an empty string.

A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`. For example, `"aabd"` is lexicographically smaller than `"aaca"` because the first position they differ is at the third letter, and `'b'` comes before `'c'`.

**Example 1:**

**Input:** lcp = \[\[4,0,2,0],[0,3,0,1],[2,0,2,0],[0,1,0,1]]

**Output:** "abab"

**Explanation:** lcp corresponds to any 4 letter string with two alternating letters. The lexicographically smallest of them is "abab".

**Example 2:**

**Input:** lcp = \[\[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,1]]

**Output:** "aaaa"

**Explanation:** lcp corresponds to any 4 letter string with a single distinct letter. The lexicographically smallest of them is "aaaa".

**Example 3:**

**Input:** lcp = \[\[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,3]]

**Output:** ""

**Explanation:** lcp[3][3] cannot be equal to 3 since word[3,...,3] consists of only a single letter; Thus, no answer exists.

**Constraints:**

*   `1 <= n == ``lcp.length ==` `lcp[i].length` `<= 1000`
*   `0 <= lcp[i][j] <= n`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun findTheString(lcp: Array<IntArray>): String {
        val n = lcp.size
        val parent = IntArray(n)
        val rank = IntArray(n)
        val chars = IntArray(n)
        val str = IntArray(n)
        for (i in 0 until n) {
            parent[i] = i
            rank[i] = 1
        }
        for (i in 0 until n) {
            for (j in i + 1 until n) {
                if (lcp[i][j] > 0) {
                    union(parent, rank, i, j)
                }
            }
        }
        var c = 0
        var par: Int
        for (i in 0 until n) {
            par = find(parent, i)
            if (chars[par] == 0) {
                chars[par] = ++c
            }
            if (c > 26) return ""
            str[i] = chars[par]
        }
        var `val`: Int
        val lcpNew = Array(n) { IntArray(n) }
        for (i in n - 1 downTo 0) {
            for (j in n - 1 downTo 0) {
                `val` = if (i + 1 < n && j + 1 < n) lcpNew[i + 1][j + 1] else 0
                `val` = if (str[i] == str[j]) 1 + `val` else 0
                lcpNew[i][j] = `val`
                if (lcpNew[i][j] != lcp[i][j]) return ""
            }
        }
        val sb = StringBuilder()
        for (e in str) {
            sb.append((e + 'a'.code - 1).toChar())
        }
        return sb.toString()
    }

    private fun find(parent: IntArray, x: Int): Int {
        return if (x == parent[x]) x else find(parent, parent[x]).also { parent[x] = it }
    }

    private fun union(parent: IntArray, rank: IntArray, u: Int, v: Int) {
        var u = u
        var v = v
        u = find(parent, u)
        v = find(parent, v)
        if (u == v) return
        if (rank[u] >= rank[v]) {
            parent[v] = u
            rank[u] += rank[v]
        } else {
            parent[u] = v
            rank[v] += rank[u]
        }
        return
    }
}
```