[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3076\. Shortest Uncommon Substring in an Array

Medium

You are given an array `arr` of size `n` consisting of **non-empty** strings.

Find a string array `answer` of size `n` such that:

*   `answer[i]` is the **shortest** substring of `arr[i]` that does **not** occur as a substring in any other string in `arr`. If multiple such substrings exist, `answer[i]` should be the lexicographically smallest. And if no such substring exists, `answer[i]` should be an empty string.

Return _the array_ `answer`.

**Example 1:**

**Input:** arr = ["cab","ad","bad","c"]

**Output:** ["ab","","ba",""]

**Explanation:** We have the following: 
- For the string "cab", the shortest substring that does not occur in any other string is either "ca" or "ab", we choose the lexicographically smaller substring, which is "ab". 
- For the string "ad", there is no substring that does not occur in any other string. 
- For the string "bad", the shortest substring that does not occur in any other string is "ba". 
- For the string "c", there is no substring that does not occur in any other string.

**Example 2:**

**Input:** arr = ["abc","bcd","abcd"]

**Output:** ["","","abcd"]

**Explanation:** We have the following: 
- For the string "abc", there is no substring that does not occur in any other string. 
- For the string "bcd", there is no substring that does not occur in any other string. 
- For the string "abcd", the shortest substring that does not occur in any other string is "abcd".

**Constraints:**

*   `n == arr.length`
*   `2 <= n <= 100`
*   `1 <= arr[i].length <= 20`
*   `arr[i]` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    private val root = Trie()

    fun shortestSubstrings(arr: Array<String>): Array<String?> {
        val n = arr.size
        for (k in 0 until n) {
            val s = arr[k]
            val cs = s.toCharArray()
            val m = cs.size
            for (i in 0 until m) {
                insert(cs, i, m, k)
            }
        }
        val ans = arrayOfNulls<String>(n)
        for (k in 0 until n) {
            val s = arr[k]
            val cs = s.toCharArray()
            val m = cs.size
            var result = ""
            var resultLen = m + 1
            for (i in 0 until m) {
                val curLen = search(
                    cs, i,
                    min(m.toDouble(), (i + resultLen).toDouble())
                        .toInt(),
                    k
                )
                if (curLen != -1) {
                    val sub = String(cs, i, curLen)
                    if (curLen < resultLen || result.compareTo(sub) > 0) {
                        result = sub
                        resultLen = curLen
                    }
                }
            }
            ans[k] = result
        }
        return ans
    }

    private fun insert(cs: CharArray, start: Int, end: Int, wordIndex: Int) {
        var curr: Trie? = root
        for (i in start until end) {
            val index = cs[i].code - 'a'.code
            if (curr!!.children[index] == null) {
                curr.children[index] = Trie()
            }
            curr = curr.children[index]
            if (curr!!.wordIndex == -1 || curr.wordIndex == wordIndex) {
                curr.wordIndex = wordIndex
            } else {
                curr.wordIndex = -2
            }
        }
    }

    private fun search(cs: CharArray, start: Int, end: Int, wordIndex: Int): Int {
        var cur: Trie? = root
        for (i in start until end) {
            val index = cs[i].code - 'a'.code
            cur = cur!!.children[index]
            if (cur!!.wordIndex == wordIndex) {
                return i - start + 1
            }
        }
        return -1
    }

    private class Trie {
        var children: Array<Trie?> = arrayOfNulls(26)
        var wordIndex: Int = -1
    }
}
```