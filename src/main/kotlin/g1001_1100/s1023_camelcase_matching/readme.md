[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1023\. Camelcase Matching

Medium

Given an array of strings `queries` and a string `pattern`, return a boolean array `answer` where `answer[i]` is `true` if `queries[i]` matches `pattern`, and `false` otherwise.

A query word `queries[i]` matches `pattern` if you can insert lowercase English letters pattern so that it equals the query. You may insert each character at any position and you may not insert any characters.

**Example 1:**

**Input:** queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FB"

**Output:** [true,false,true,true,false]

**Explanation:** "FooBar" can be generated like this "F" + "oo" + "B" + "ar". "FootBall" can be generated like this "F" + "oot" + "B" + "all". "FrameBuffer" can be generated like this "F" + "rame" + "B" + "uffer".

**Example 2:**

**Input:** queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBa"

**Output:** [true,false,true,false,false]

**Explanation:** "FooBar" can be generated like this "Fo" + "o" + "Ba" + "r". "FootBall" can be generated like this "Fo" + "ot" + "Ba" + "ll".

**Example 3:**

**Input:** queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBaT"

**Output:** [false,true,false,false,false]

**Explanation:** "FooBarTest" can be generated like this "Fo" + "o" + "Ba" + "r" + "T" + "est".

**Constraints:**

*   `1 <= pattern.length, queries.length <= 100`
*   `1 <= queries[i].length <= 100`
*   `queries[i]` and `pattern` consist of English letters.

## Solution

```kotlin
import java.util.LinkedList

class Solution {
    fun camelMatch(queries: Array<String>, pattern: String): List<Boolean> {
        val ret: MutableList<Boolean> = LinkedList()
        for (query in queries) {
            ret.add(check(query, pattern))
        }
        return ret
    }

    private fun check(query: String, pattern: String): Boolean {
        val patternLen = pattern.length
        var patternPos = 0
        var uppercaseCount = 0
        for (element in query) {
            val c = element
            if (Character.isUpperCase(c)) {
                if (patternPos < patternLen && c != pattern[patternPos]) {
                    return false
                }
                uppercaseCount++
                if (uppercaseCount > patternLen) {
                    return false
                }
                patternPos++
            } else {
                if (patternPos < patternLen && c == pattern[patternPos]) {
                    patternPos++
                }
            }
        }
        return patternPos == patternLen
    }
}
```