[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1170\. Compare Strings by Frequency of the Smallest Character

Medium

Let the function `f(s)` be the **frequency of the lexicographically smallest character** in a non-empty string `s`. For example, if `s = "dcce"` then `f(s) = 2` because the lexicographically smallest character is `'c'`, which has a frequency of 2.

You are given an array of strings `words` and another array of query strings `queries`. For each query `queries[i]`, count the **number of words** in `words` such that `f(queries[i])` < `f(W)` for each `W` in `words`.

Return _an integer array_ `answer`_, where each_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query_.

**Example 1:**

**Input:** queries = ["cbd"], words = ["zaaaz"]

**Output:** [1]

**Explanation:** On the first query we have f("cbd") = 1, f("zaaaz") = 3 so f("cbd") < f("zaaaz").

**Example 2:**

**Input:** queries = ["bbb","cc"], words = ["a","aa","aaa","aaaa"]

**Output:** [1,2]

**Explanation:** On the first query only f("bbb") < f("aaaa"). On the second query both f("aaa") and f("aaaa") are both > f("cc").

**Constraints:**

*   `1 <= queries.length <= 2000`
*   `1 <= words.length <= 2000`
*   `1 <= queries[i].length, words[i].length <= 10`
*   `queries[i][j]`, `words[i][j]` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun numSmallerByFrequency(queries: Array<String>, words: Array<String>): IntArray {
        val queriesMinFrequecies = IntArray(queries.size)
        for (i in queries.indices) {
            queriesMinFrequecies[i] = computeLowestFrequency(queries[i])
        }
        val wordsMinFrequecies = IntArray(words.size)
        for (i in words.indices) {
            wordsMinFrequecies[i] = computeLowestFrequency(words[i])
        }
        wordsMinFrequecies.sort()
        val result = IntArray(queries.size)
        for (i in result.indices) {
            result[i] = search(wordsMinFrequecies, queriesMinFrequecies[i])
        }
        return result
    }

    private fun search(nums: IntArray, target: Int): Int {
        var count = 0
        for (i in nums.indices.reversed()) {
            if (nums[i] > target) {
                count++
            } else {
                break
            }
        }
        return count
    }

    private fun computeLowestFrequency(string: String): Int {
        val str = string.toCharArray()
        str.sort()
        val sortedString = String(str)
        var frequency = 1
        for (i in 1 until sortedString.length) {
            if (sortedString[i] == sortedString[0]) {
                frequency++
            } else {
                break
            }
        }
        return frequency
    }
}
```