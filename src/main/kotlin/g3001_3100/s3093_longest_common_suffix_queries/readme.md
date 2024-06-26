[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3093\. Longest Common Suffix Queries

Hard

You are given two arrays of strings `wordsContainer` and `wordsQuery`.

For each `wordsQuery[i]`, you need to find a string from `wordsContainer` that has the **longest common suffix** with `wordsQuery[i]`. If there are two or more strings in `wordsContainer` that share the longest common suffix, find the string that is the **smallest** in length. If there are two or more such strings that have the **same** smallest length, find the one that occurred **earlier** in `wordsContainer`.

Return _an array of integers_ `ans`_, where_ `ans[i]` _is the index of the string in_ `wordsContainer` _that has the **longest common suffix** with_ `wordsQuery[i]`_._

**Example 1:**

**Input:** wordsContainer = ["abcd","bcd","xbcd"], wordsQuery = ["cd","bcd","xyz"]

**Output:** [1,1,1]

**Explanation:**

Let's look at each `wordsQuery[i]` separately:

*   For `wordsQuery[0] = "cd"`, strings from `wordsContainer` that share the longest common suffix `"cd"` are at indices 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.
*   For `wordsQuery[1] = "bcd"`, strings from `wordsContainer` that share the longest common suffix `"bcd"` are at indices 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.
*   For `wordsQuery[2] = "xyz"`, there is no string from `wordsContainer` that shares a common suffix. Hence the longest common suffix is `""`, that is shared with strings at index 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.

**Example 2:**

**Input:** wordsContainer = ["abcdefgh","poiuygh","ghghgh"], wordsQuery = ["gh","acbfgh","acbfegh"]

**Output:** [2,0,2]

**Explanation:**

Let's look at each `wordsQuery[i]` separately:

*   For `wordsQuery[0] = "gh"`, strings from `wordsContainer` that share the longest common suffix `"gh"` are at indices 0, 1, and 2. Among these, the answer is the string at index 2 because it has the shortest length of 6.
*   For `wordsQuery[1] = "acbfgh"`, only the string at index 0 shares the longest common suffix `"fgh"`. Hence it is the answer, even though the string at index 2 is shorter.
*   For `wordsQuery[2] = "acbfegh"`, strings from `wordsContainer` that share the longest common suffix `"gh"` are at indices 0, 1, and 2. Among these, the answer is the string at index 2 because it has the shortest length of 6.

**Constraints:**

*   <code>1 <= wordsContainer.length, wordsQuery.length <= 10<sup>4</sup></code>
*   <code>1 <= wordsContainer[i].length <= 5 * 10<sup>3</sup></code>
*   <code>1 <= wordsQuery[i].length <= 5 * 10<sup>3</sup></code>
*   `wordsContainer[i]` consists only of lowercase English letters.
*   `wordsQuery[i]` consists only of lowercase English letters.
*   Sum of `wordsContainer[i].length` is at most <code>5 * 10<sup>5</sup></code>.
*   Sum of `wordsQuery[i].length` is at most <code>5 * 10<sup>5</sup></code>.

## Solution

```kotlin
class Solution {
    fun stringIndices(wc: Array<String>, wq: Array<String>): IntArray {
        var minLength = wc[0].length
        var minIndex = 0
        val n = wc.size
        val m = wq.size
        for (i in 0 until n) {
            if (minLength > wc[i].length) {
                minLength = wc[i].length
                minIndex = i
            }
        }
        val root = Trie(minIndex)
        for (i in 0 until n) {
            var curr: Trie? = root
            for (j in wc[i].length - 1 downTo 0) {
                val ch = wc[i][j]
                if (curr!!.has(ch)) {
                    val next = curr.get(ch)
                    if (wc[next!!.index].length > wc[i].length) {
                        next.index = i
                    }
                    curr = next
                } else {
                    curr.put(ch, i)
                    curr = curr.get(ch)
                }
            }
        }
        val ans = IntArray(m)
        for (i in 0 until m) {
            var curr: Trie? = root
            for (j in wq[i].length - 1 downTo 0) {
                val ch = wq[i][j]
                if (curr!!.has(ch)) {
                    curr = curr.get(ch)
                } else {
                    break
                }
            }
            ans[i] = curr!!.index
        }
        return ans
    }

    private class Trie(var index: Int) {
        var ch: Array<Trie?> = arrayOfNulls(26)

        fun get(ch: Char): Trie? {
            return this.ch[ch.code - 'a'.code]
        }

        fun has(ch: Char): Boolean {
            return this.ch[ch.code - 'a'.code] != null
        }

        fun put(ch: Char, index: Int) {
            this.ch[ch.code - 'a'.code] = Trie(index)
        }
    }
}
```