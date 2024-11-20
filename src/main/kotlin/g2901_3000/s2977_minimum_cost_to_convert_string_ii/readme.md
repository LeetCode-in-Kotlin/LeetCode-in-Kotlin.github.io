[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2977\. Minimum Cost to Convert String II

Hard

You are given two **0-indexed** strings `source` and `target`, both of length `n` and consisting of **lowercase** English characters. You are also given two **0-indexed** string arrays `original` and `changed`, and an integer array `cost`, where `cost[i]` represents the cost of converting the string `original[i]` to the string `changed[i]`.

You start with the string `source`. In one operation, you can pick a **substring** `x` from the string, and change it to `y` at a cost of `z` **if** there exists **any** index `j` such that `cost[j] == z`, `original[j] == x`, and `changed[j] == y`. You are allowed to do **any** number of operations, but any pair of operations must satisfy **either** of these two conditions:

*   The substrings picked in the operations are `source[a..b]` and `source[c..d]` with either `b < c` **or** `d < a`. In other words, the indices picked in both operations are **disjoint**.
*   The substrings picked in the operations are `source[a..b]` and `source[c..d]` with `a == c` **and** `b == d`. In other words, the indices picked in both operations are **identical**.

Return _the **minimum** cost to convert the string_ `source` _to the string_ `target` _using **any** number of operations_. _If it is impossible to convert_ `source` _to_ `target`, _return_ `-1`.

**Note** that there may exist indices `i`, `j` such that `original[j] == original[i]` and `changed[j] == changed[i]`.

**Example 1:**

**Input:** source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]

**Output:** 28

**Explanation:** To convert "abcd" to "acbe", do the following operations: 
- Change substring source[1..1] from "b" to "c" at a cost of 5. 
- Change substring source[2..2] from "c" to "e" at a cost of 1. 
- Change substring source[2..2] from "e" to "b" at a cost of 2. 
- Change substring source[3..3] from "d" to "e" at a cost of 20. 

The total cost incurred is 5 + 1 + 2 + 20 = 28. 

It can be shown that this is the minimum possible cost.

**Example 2:**

**Input:** source = "abcdefgh", target = "acdeeghh", original = ["bcd","fgh","thh"], changed = ["cde","thh","ghh"], cost = [1,3,5]

**Output:** 9

**Explanation:** To convert "abcdefgh" to "acdeeghh", do the following operations: 
- Change substring source[1..3] from "bcd" to "cde" at a cost of 1. 
- Change substring source[5..7] from "fgh" to "thh" at a cost of 3. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation. 
- Change substring source[5..7] from "thh" to "ghh" at a cost of 5. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation, and identical with indices picked in the second operation. 

The total cost incurred is 1 + 3 + 5 = 9. 

It can be shown that this is the minimum possible cost.

**Example 3:**

**Input:** source = "abcdefgh", target = "addddddd", original = ["bcd","defgh"], changed = ["ddd","ddddd"], cost = [100,1578]

**Output:** -1

**Explanation:** It is impossible to convert "abcdefgh" to "addddddd".

If you select substring source[1..3] as the first operation to change "abcdefgh" to "adddefgh", you cannot select substring source[3..7] as the second operation because it has a common index, 3, with the first operation. 

If you select substring source[3..7] as the first operation to change "abcdefgh" to "abcddddd", you cannot select substring source[1..3] as the second operation because it has a common index, 3, with the first operation.

**Constraints:**

*   `1 <= source.length == target.length <= 1000`
*   `source`, `target` consist only of lowercase English characters.
*   `1 <= cost.length == original.length == changed.length <= 100`
*   `1 <= original[i].length == changed[i].length <= source.length`
*   `original[i]`, `changed[i]` consist only of lowercase English characters.
*   `original[i] != changed[i]`
*   <code>1 <= cost[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimumCost(
        source: String,
        target: String,
        original: Array<String>,
        changed: Array<String>,
        cost: IntArray,
    ): Long {
        val index = HashMap<String, Int>()
        for (o in original) {
            if (!index.containsKey(o)) {
                index[o] = index.size
            }
        }
        for (c in changed) {
            if (!index.containsKey(c)) {
                index[c] = index.size
            }
        }
        val dis = Array(index.size) { LongArray(index.size) }
        for (i in dis.indices) {
            dis[i].fill(Long.MAX_VALUE)
            dis[i][i] = 0
        }
        for (i in cost.indices) {
            dis[index[original[i]]!!][index[changed[i]]!!] =
                min(dis[index[original[i]]!!][index[changed[i]]!!], cost[i].toLong())
        }
        for (k in dis.indices) {
            for (i in dis.indices) {
                if (dis[i][k] < Long.MAX_VALUE) {
                    for (j in dis.indices) {
                        if (dis[k][j] < Long.MAX_VALUE) {
                            dis[i][j] = min(dis[i][j], (dis[i][k] + dis[k][j]))
                        }
                    }
                }
            }
        }
        val set = HashSet<Int>()
        for (o in original) {
            set.add(o.length)
        }
        val dp = LongArray(target.length + 1)
        dp.fill(Long.MAX_VALUE)
        dp[0] = 0L
        for (i in target.indices) {
            if (dp[i] == Long.MAX_VALUE) {
                continue
            }
            if (target[i] == source[i]) {
                dp[i + 1] = min(dp[i + 1], dp[i])
            }
            for (t in set) {
                if (i + t >= dp.size) {
                    continue
                }
                val c1 = index.getOrDefault(source.substring(i, i + t), -1)
                val c2 = index.getOrDefault(target.substring(i, i + t), -1)
                if (c1 >= 0 && c2 >= 0 && dis[c1][c2] < Long.MAX_VALUE) {
                    dp[i + t] = min(dp[i + t], (dp[i] + dis[c1][c2]))
                }
            }
        }
        return if (dp[dp.size - 1] == Long.MAX_VALUE) -1L else dp[dp.size - 1]
    }
}
```