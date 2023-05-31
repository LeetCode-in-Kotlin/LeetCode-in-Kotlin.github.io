[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1061\. Lexicographically Smallest Equivalent String

Medium

You are given two strings of the same length `s1` and `s2` and a string `baseStr`.

We say `s1[i]` and `s2[i]` are equivalent characters.

*   For example, if `s1 = "abc"` and `s2 = "cde"`, then we have `'a' == 'c'`, `'b' == 'd'`, and `'c' == 'e'`.

Equivalent characters follow the usual rules of any equivalence relation:

*   **Reflexivity:** `'a' == 'a'`.
*   **Symmetry:** `'a' == 'b'` implies `'b' == 'a'`.
*   **Transitivity:** `'a' == 'b'` and `'b' == 'c'` implies `'a' == 'c'`.

For example, given the equivalency information from `s1 = "abc"` and `s2 = "cde"`, `"acd"` and `"aab"` are equivalent strings of `baseStr = "eed"`, and `"aab"` is the lexicographically smallest equivalent string of `baseStr`.

Return _the lexicographically smallest equivalent string of_ `baseStr` _by using the equivalency information from_ `s1` _and_ `s2`.

**Example 1:**

**Input:** s1 = "parker", s2 = "morris", baseStr = "parser"

**Output:** "makkek"

**Explanation:** Based on the equivalency information in s1 and s2, we can group their characters as [m,p], [a,o], [k,r,s], [e,i]. 

The characters in each group are equivalent and sorted in lexicographical order.

So the answer is "makkek".

**Example 2:**

**Input:** s1 = "hello", s2 = "world", baseStr = "hold"

**Output:** "hdld"

**Explanation:** Based on the equivalency information in s1 and s2, we can group their characters as [h,w], [d,e,o], [l,r]. 

So only the second letter 'o' in baseStr is changed to 'd', the answer is "hdld".

**Example 3:**

**Input:** s1 = "leetcode", s2 = "programs", baseStr = "sourcecode"

**Output:** "aauaaaaada"

**Explanation:** We group the equivalent characters in s1 and s2 as [a,o,e,r,s,c], [l,p], [g,t] and [d,m], thus all letters in baseStr except 'u' and 'd' are transformed to 'a', the answer is "aauaaaaada".

**Constraints:**

*   `1 <= s1.length, s2.length, baseStr <= 1000`
*   `s1.length == s2.length`
*   `s1`, `s2`, and `baseStr` consist of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    lateinit var parent: IntArray

    fun smallestEquivalentString(s1: String, s2: String, baseStr: String): String? {
        parent = IntArray(26)
        val n = s1.length
        var result = ""
        for (i in 0..25) parent[i] = i
        for (i in 0 until n) {
            union(s1[i].code - 'a'.code, s2[i].code - 'a'.code)
        }
        val base = 'a'.code
        for (element in baseStr) {
            result += (base + find(element.code - 'a'.code)).toChar()
        }
        return result
    }

    private fun union(a: Int, b: Int) {
        val parentA = find(a)
        val parentB = find(b)
        if (parentA != parentB) {
            if (parentA < parentB) {
                parent[parentB] = parentA
            } else {
                parent[parentA] = parentB
            }
        }
    }

    private fun find(x: Int): Int {
        var x = x
        while (parent[x] != x) {
            x = parent[x]
        }
        return x
    }
}
```