[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3211\. Generate Binary Strings Without Adjacent Zeros

Medium

You are given a positive integer `n`.

A binary string `x` is **valid** if all substrings of `x` of length 2 contain **at least** one `"1"`.

Return all **valid** strings with length `n`**,** in _any_ order.

**Example 1:**

**Input:** n = 3

**Output:** ["010","011","101","110","111"]

**Explanation:**

The valid strings of length 3 are: `"010"`, `"011"`, `"101"`, `"110"`, and `"111"`.

**Example 2:**

**Input:** n = 1

**Output:** ["0","1"]

**Explanation:**

The valid strings of length 1 are: `"0"` and `"1"`.

**Constraints:**

*   `1 <= n <= 18`

## Solution

```kotlin
class Solution {
    fun validStrings(n: Int): List<String> {
        val strings: MutableList<String> = ArrayList()
        dfs(n, StringBuilder(), strings)
        return strings
    }

    private fun dfs(n: Int, build: StringBuilder, strings: MutableList<String>) {
        if (build.length == n) {
            strings.add(build.toString())
            return
        }
        // need to add a one
        if (build.isNotEmpty() && build[build.length - 1] == '0') {
            build.append('1')
            dfs(n, build, strings)
            // undo for backtracking
            build.setLength(build.length - 1)
            return
        }
        // choose to append a one
        build.append('1')
        dfs(n, build, strings)
        build.setLength(build.length - 1)
        // choose to append a zero
        build.append('0')
        dfs(n, build, strings)
        build.setLength(build.length - 1)
    }
}
```