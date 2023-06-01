[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1128\. Number of Equivalent Domino Pairs

Easy

Given a list of `dominoes`, `dominoes[i] = [a, b]` is **equivalent to** `dominoes[j] = [c, d]` if and only if either (`a == c` and `b == d`), or (`a == d` and `b == c`) - that is, one domino can be rotated to be equal to another domino.

Return _the number of pairs_ `(i, j)` _for which_ `0 <= i < j < dominoes.length`_, and_ `dominoes[i]` _is **equivalent to**_ `dominoes[j]`.

**Example 1:**

**Input:** dominoes = \[\[1,2],[2,1],[3,4],[5,6]]

**Output:** 1

**Example 2:**

**Input:** dominoes = \[\[1,2],[1,2],[1,1],[1,2],[2,2]]

**Output:** 3

**Constraints:**

*   <code>1 <= dominoes.length <= 4 * 10<sup>4</sup></code>
*   `dominoes[i].length == 2`
*   `1 <= dominoes[i][j] <= 9`

## Solution

```kotlin
class Solution {
    fun numEquivDominoPairs(dominoes: Array<IntArray>): Int {
        val map: MutableMap<Int, Int> = HashMap()
        var count = 0
        for (dominoe in dominoes) {
            val smaller = Math.min(dominoe[0], dominoe[1])
            val bigger = Math.max(dominoe[0], dominoe[1])
            val key = smaller * 10 + bigger
            count += map.getOrDefault(key, 0)
            map[key] = map.getOrDefault(key, 0) + 1
        }
        return count
    }
}
```