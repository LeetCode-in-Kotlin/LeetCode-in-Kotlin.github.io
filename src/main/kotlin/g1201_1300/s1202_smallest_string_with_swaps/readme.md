[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1202\. Smallest String With Swaps

Medium

You are given a string `s`, and an array of pairs of indices in the string `pairs` where `pairs[i] = [a, b]` indicates 2 indices(0-indexed) of the string.

You can swap the characters at any pair of indices in the given `pairs` **any number of times**.

Return the lexicographically smallest string that `s` can be changed to after using the swaps.

**Example 1:**

**Input:** s = "dcab", pairs = \[\[0,3],[1,2]]

**Output:** "bacd" **Explaination:** 

Swap s[0] and s[3], s = "bcad" 

Swap s[1] and s[2], s = "bacd"

**Example 2:**

**Input:** s = "dcab", pairs = \[\[0,3],[1,2],[0,2]]

**Output:** "abcd" **Explaination:**  

Swap s[0] and s[3], s = "bcad" 

Swap s[0] and s[2], s = "acbd" 

Swap s[1] and s[2], s = "abcd"

**Example 3:**

**Input:** s = "cba", pairs = \[\[0,1],[1,2]]

**Output:** "abc" **Explaination:**  

Swap s[0] and s[1], s = "bca" 

Swap s[1] and s[2], s = "bac" 

Swap s[0] and s[1], s = "abc"

**Constraints:**

*   `1 <= s.length <= 10^5`
*   `0 <= pairs.length <= 10^5`
*   `0 <= pairs[i][0], pairs[i][1] < s.length`
*   `s` only contains lower case English letters.

## Solution

```kotlin
class Solution {
    fun smallestStringWithSwaps(s: String, pairs: List<List<Int>>): String {
        val uf = UF(s.length)
        for (p in pairs) {
            uf.union(p[0], p[1])
        }
        val freqMapPerRoot: MutableMap<Int, IntArray> = HashMap()
        for (i in s.indices) {
            freqMapPerRoot.computeIfAbsent(uf.find(i)) { IntArray(26) }[s[i].code - 'a'.code]++
        }
        val ans = CharArray(s.length)
        for (i in ans.indices) {
            val css = freqMapPerRoot[uf.find(i)]
            for (j in css!!.indices) {
                if (css[j] > 0) {
                    ans[i] = (j + 'a'.code).toChar()
                    css[j]--
                    break
                }
            }
        }
        return String(ans)
    }

    internal class UF(n: Int) {
        var root: IntArray
        var rank: IntArray

        init {
            root = IntArray(n)
            rank = IntArray(n)
            for (i in 0 until n) {
                root[i] = i
                rank[i] = 1
            }
        }

        fun find(u: Int): Int {
            if (u == root[u]) {
                return u
            }
            root[u] = find(root[u])
            return root[u]
        }

        fun union(u: Int, v: Int) {
            val ru = find(root[u])
            val rv = find(root[v])
            if (ru != rv) {
                if (rank[ru] < rank[rv]) {
                    root[ru] = root[rv]
                } else if (rank[ru] > rank[rv]) {
                    root[rv] = root[ru]
                } else {
                    root[rv] = root[ru]
                    rank[ru]++
                }
            }
        }
    }
}
```