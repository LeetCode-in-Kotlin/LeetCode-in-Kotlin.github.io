[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2559\. Count Vowel Strings in Ranges

Medium

You are given a **0-indexed** array of strings `words` and a 2D array of integers `queries`.

Each query <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> asks us to find the number of strings present in the range <code>l<sub>i</sub></code> to <code>r<sub>i</sub></code> (both **inclusive**) of `words` that start and end with a vowel.

Return _an array_ `ans` _of size_ `queries.length`_, where_ `ans[i]` _is the answer to the_ `i`<sup>th</sup> _query_.

**Note** that the vowel letters are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**

**Input:** words = ["aba","bcb","ece","aa","e"], queries = \[\[0,2],[1,4],[1,1]]

**Output:** [2,3,0]

**Explanation:** The strings starting and ending with a vowel are "aba", "ece", "aa" and "e". 

The answer to the query [0,2] is 2 (strings "aba" and "ece"). 

to query [1,4] is 3 (strings "ece", "aa", "e"). 

to query [1,1] is 0. 

We return [2,3,0].

**Example 2:**

**Input:** words = ["a","e","i"], queries = \[\[0,2],[0,1],[2,2]]

**Output:** [3,2,1]

**Explanation:** Every string satisfies the conditions, so we return [3,2,1].

**Constraints:**

*   <code>1 <= words.length <= 10<sup>5</sup></code>
*   `1 <= words[i].length <= 40`
*   `words[i]` consists only of lowercase English letters.
*   <code>sum(words[i].length) <= 3 * 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < words.length</code>

## Solution

```kotlin
class Solution {
    fun vowelStrings(words: Array<String>, queries: Array<IntArray>): IntArray {
        val vowels = HashSet(listOf('a', 'e', 'i', 'o', 'u'))
        val n = words.size
        val validWords = IntArray(n)
        for (i in 0 until n) {
            val startChar = words[i][0]
            val endChar = words[i][words[i].length - 1]
            validWords[i] = if (vowels.contains(startChar) && vowels.contains(endChar)) 1 else 0
        }
        val prefix = IntArray(n)
        prefix[0] = validWords[0]
        for (i in 1 until n) {
            prefix[i] = prefix[i - 1] + validWords[i]
        }
        val output = IntArray(queries.size)
        for (i in queries.indices) {
            val start = queries[i][0]
            val end = queries[i][1]
            output[i] = if (start == 0) prefix[end] else prefix[end] - prefix[start - 1]
        }
        return output
    }
}
```