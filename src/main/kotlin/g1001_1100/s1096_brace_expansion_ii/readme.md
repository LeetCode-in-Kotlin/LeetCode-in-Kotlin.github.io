[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1096\. Brace Expansion II

Hard

Under the grammar given below, strings can represent a set of lowercase words. Let `R(expr)` denote the set of words the expression represents.

The grammar can best be understood through simple examples:

*   Single letters represent a singleton set containing that word.
    *   `R("a") = {"a"}`
    *   `R("w") = {"w"}`
*   When we take a comma-delimited list of two or more expressions, we take the union of possibilities.
    *   `R("{a,b,c}") = {"a","b","c"}`
    *   `R("{ {a,b},{b,c}}") = {"a","b","c"}` (notice the final set only contains each word at most once)
*   When we concatenate two expressions, we take the set of possible concatenations between two words where the first word comes from the first expression and the second word comes from the second expression.
    *   `R("{a,b}{c,d}") = {"ac","ad","bc","bd"}`
    *   `R("a{b,c}{d,e}f{g,h}") = {"abdfg", "abdfh", "abefg", "abefh", "acdfg", "acdfh", "acefg", "acefh"}`

Formally, the three rules for our grammar:

*   For every lowercase letter `x`, we have `R(x) = {x}`.
*   For expressions <code>e<sub>1</sub>, e<sub>2</sub>, ... , e<sub>k</sub></code> with `k >= 2`, we have <code>R({e<sub>1</sub>, e<sub>2</sub>, ...}) = R(e<sub>1</sub>) ∪ R(e<sub>2</sub>) ∪ ...</code>
*   For expressions <code>e<sub>1</sub></code> and <code>e<sub>2</sub></code>, we have <code>R(e<sub>1</sub> + e<sub>2</sub>) = {a + b for (a, b) in R(e<sub>1</sub>) × R(e<sub>2</sub>)}</code>, where `+` denotes concatenation, and `×` denotes the cartesian product.

Given an expression representing a set of words under the given grammar, return _the sorted list of words that the expression represents_.

**Example 1:**

**Input:** expression = "{a,b}{c,{d,e}}"

**Output:** ["ac","ad","ae","bc","bd","be"]

**Example 2:**

**Input:** expression = "{ {a,z},a{b,c},{ab,z}}"

**Output:** ["a","ab","ac","z"]

**Explanation:** Each distinct word is written only once in the final answer.

**Constraints:**

*   `1 <= expression.length <= 60`
*   `expression[i]` consists of `'{'`, `'}'`, `','`or lowercase English letters.
*   The given `expression` represents a set of words based on the grammar given in the description.

## Solution

```kotlin
class Solution {
    fun braceExpansionII(expression: String): List<String> {
        val res = flatten(expression)
        val sorted: MutableList<String> = ArrayList(res)
        sorted.sort()
        return sorted
    }

    private fun flatten(expression: String): Set<String> {
        val res: MutableSet<String> = HashSet()
        // A temp set to store cartesian product results.
        var curSet: MutableSet<String> = HashSet()
        var idx = 0
        while (idx < expression.length) {
            if (expression[idx] == '{') {
                // end will be the index of matching "}"
                val end = findClosingBrace(expression, idx)
                val set = flatten(expression.substring(idx + 1, end))
                curSet = concatenateSet(curSet, set)
                idx = end + 1
            } else if (Character.isLowerCase(expression[idx])) {
                // Create set with single element
                val set: Set<String> = HashSet(
                    listOf(
                        expression[idx].toString(),
                    ),
                )
                curSet = concatenateSet(curSet, set)
                idx++
            } else if (expression[idx] == ',') {
                res.addAll(curSet)
                curSet.clear()
                idx++
            }
        }
        // Don't forget!
        res.addAll(curSet)
        return res
    }

    private fun concatenateSet(set1: Set<String>, set2: Set<String>): MutableSet<String> {
        if (set1.isEmpty() || set2.isEmpty()) {
            return if (set2.isNotEmpty()) HashSet(set2) else HashSet(set1)
        }
        val res: MutableSet<String> = HashSet()
        for (s1 in set1) {
            for (s2 in set2) {
                res.add(s1 + s2)
            }
        }
        return res
    }

    private fun findClosingBrace(expression: String, start: Int): Int {
        var count = 0
        var idx = start
        while (idx < expression.length) {
            if (expression[idx] == '{') {
                count++
            } else if (expression[idx] == '}') {
                count--
            }
            if (count == 0) {
                break
            }
            idx++
        }
        return idx
    }
}
```