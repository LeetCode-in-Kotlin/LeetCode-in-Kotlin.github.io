[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1048\. Longest String Chain

Medium

You are given an array of `words` where each word consists of lowercase English letters.

<code>word<sub>A</sub></code> is a **predecessor** of <code>word<sub>B</sub></code> if and only if we can insert **exactly one** letter anywhere in <code>word<sub>A</sub></code> **without changing the order of the other characters** to make it equal to <code>word<sub>B</sub></code>.

*   For example, `"abc"` is a **predecessor** of <code>"ab<ins>a</ins>c"</code>, while `"cba"` is not a **predecessor** of `"bcad"`.

A **word chain** is a sequence of words <code>[word<sub>1</sub>, word<sub>2</sub>, ..., word<sub>k</sub>]</code> with `k >= 1`, where <code>word<sub>1</sub></code> is a **predecessor** of <code>word<sub>2</sub></code>, <code>word<sub>2</sub></code> is a **predecessor** of <code>word<sub>3</sub></code>, and so on. A single word is trivially a **word chain** with `k == 1`.

Return _the **length** of the **longest possible word chain** with words chosen from the given list of_ `words`.

**Example 1:**

**Input:** words = ["a","b","ba","bca","bda","bdca"]

**Output:** 4

**Explanation:**: One of the longest word chains is ["a","<ins>b</ins>a","b<ins>d</ins>a","bd<ins>c</ins>a"].

**Example 2:**

**Input:** words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]

**Output:** 5

**Explanation:** All the words can be put in a word chain ["xb", "xb<ins>c</ins>", "<ins>c</ins>xbc", "<ins>p</ins>cxbc", "pcxbc<ins>f</ins>"].

**Example 3:**

**Input:** words = ["abcd","dbqca"]

**Output:** 1

**Explanation:** The trivial word chain ["abcd"] is one of the longest word chains. ["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.

**Constraints:**

*   `1 <= words.length <= 1000`
*   `1 <= words[i].length <= 16`
*   `words[i]` only consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun longestStrChain(words: Array<String>): Int {
        val lenStr = arrayOfNulls<MutableList<String>>(20)
        for (word in words) {
            val len = word.length
            if (lenStr[len] == null) {
                lenStr[len] = ArrayList()
            }
            lenStr[len]!!.add(word)
        }
        val longest: MutableMap<String?, Int?> = HashMap()
        var max = 0
        for (s in words) {
            max = findLongest(s, lenStr, longest).coerceAtLeast(max)
        }
        return max
    }

    private fun findLongest(
        word: String,
        lenStr: Array<MutableList<String>?>,
        longest: MutableMap<String?, Int?>,
    ): Int {
        if (longest.containsKey(word)) {
            return longest[word]!!
        }
        val len = word.length
        val words: List<String>? = lenStr[len + 1]
        if (words == null) {
            longest[word] = 1
            return 1
        }
        var max = 0
        var i: Int
        var j: Int
        for (w in words) {
            i = 0
            j = 0
            while (i < len && j - i <= 1) {
                if (word[i] == w[j++]) {
                    ++i
                }
            }
            if (j - i <= 1) {
                max = findLongest(w, lenStr, longest).coerceAtLeast(max)
            }
        }
        ++max
        longest[word] = max
        return max
    }
}
```