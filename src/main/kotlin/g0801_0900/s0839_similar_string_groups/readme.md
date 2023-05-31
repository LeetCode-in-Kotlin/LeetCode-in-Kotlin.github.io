[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 839\. Similar String Groups

Hard

Two strings `X` and `Y` are similar if we can swap two letters (in different positions) of `X`, so that it equals `Y`. Also two strings `X` and `Y` are similar if they are equal.

For example, `"tars"` and `"rats"` are similar (swapping at positions `0` and `2`), and `"rats"` and `"arts"` are similar, but `"star"` is not similar to `"tars"`, `"rats"`, or `"arts"`.

Together, these form two connected groups by similarity: `{"tars", "rats", "arts"}` and `{"star"}`. Notice that `"tars"` and `"arts"` are in the same group even though they are not similar. Formally, each group is such that a word is in the group if and only if it is similar to at least one other word in the group.

We are given a list `strs` of strings where every string in `strs` is an anagram of every other string in `strs`. How many groups are there?

**Example 1:**

**Input:** strs = ["tars","rats","arts","star"]

**Output:** 2

**Example 2:**

**Input:** strs = ["omv","ovm"]

**Output:** 1

**Constraints:**

*   `1 <= strs.length <= 300`
*   `1 <= strs[i].length <= 300`
*   `strs[i]` consists of lowercase letters only.
*   All words in `strs` have the same length and are anagrams of each other.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun numSimilarGroups(strs: Array<String>): Int {
        val visited = BooleanArray(strs.size)
        var res = 0
        for (i in strs.indices) {
            if (!visited[i]) {
                res++
                bfs(i, visited, strs)
            }
        }
        return res
    }

    private fun bfs(i: Int, visited: BooleanArray, strs: Array<String>) {
        val qu: Queue<String> = LinkedList()
        qu.add(strs[i])
        visited[i] = true
        while (qu.isNotEmpty()) {
            val s = qu.poll()
            for (j in strs.indices) {
                if (visited[j]) {
                    continue
                }
                if (isSimilar(s, strs[j])) {
                    visited[j] = true
                    qu.add(strs[j])
                }
            }
        }
    }

    private fun isSimilar(s1: String, s2: String): Boolean {
        var c1: Char? = null
        var c2: Char? = null
        var mismatchCount = 0
        for (i in s1.indices) {
            if (s1[i] == s2[i]) {
                continue
            }
            mismatchCount++
            if (c1 == null) {
                c1 = s1[i]
                c2 = s2[i]
            } else if (s2[i] != c1 || s1[i] != c2) {
                return false
            } else if (mismatchCount > 2) {
                return false
            }
        }
        return true
    }
}
```