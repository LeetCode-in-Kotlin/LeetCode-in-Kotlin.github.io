[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2379\. Minimum Recolors to Get K Consecutive Black Blocks

Easy

You are given a **0-indexed** string `blocks` of length `n`, where `blocks[i]` is either `'W'` or `'B'`, representing the color of the <code>i<sup>th</sup></code> block. The characters `'W'` and `'B'` denote the colors white and black, respectively.

You are also given an integer `k`, which is the desired number of **consecutive** black blocks.

In one operation, you can **recolor** a white block such that it becomes a black block.

Return _the **minimum** number of operations needed such that there is at least **one** occurrence of_ `k` _consecutive black blocks._

**Example 1:**

**Input:** blocks = "WBBWWBBWBW", k = 7

**Output:** 3

**Explanation:**

One way to achieve 7 consecutive black blocks is to recolor the 0th, 3rd, and 4th blocks so that blocks = "BBBBBBBWBW".

It can be shown that there is no way to achieve 7 consecutive black blocks in less than 3 operations.

Therefore, we return 3.

**Example 2:**

**Input:** blocks = "WBWBBBW", k = 2

**Output:** 0

**Explanation:**

No changes need to be made, since 2 consecutive black blocks already exist.

Therefore, we return 0. 

**Constraints:**

*   `n == blocks.length`
*   `1 <= n <= 100`
*   `blocks[i]` is either `'W'` or `'B'`.
*   `1 <= k <= n`

## Solution

```kotlin
class Solution {
    fun minimumRecolors(blocks: String, k: Int): Int {
        val n = blocks.length
        var ans: Int
        var i: Int
        var cur = 0
        i = 0
        while (i < k) {
            if (blocks[i] == 'W') {
                cur++
            }
            i++
        }
        ans = cur
        i = k
        while (i < n) {
            if (blocks[i] == 'W') {
                cur++
            }
            if (blocks[i - k] == 'W') {
                cur--
            }
            ans = Math.min(ans, cur)
            i++
        }
        return ans
    }
}
```