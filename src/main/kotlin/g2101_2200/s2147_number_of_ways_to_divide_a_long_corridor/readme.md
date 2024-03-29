[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2147\. Number of Ways to Divide a Long Corridor

Hard

Along a long library corridor, there is a line of seats and decorative plants. You are given a **0-indexed** string `corridor` of length `n` consisting of letters `'S'` and `'P'` where each `'S'` represents a seat and each `'P'` represents a plant.

One room divider has **already** been installed to the left of index `0`, and **another** to the right of index `n - 1`. Additional room dividers can be installed. For each position between indices `i - 1` and `i` (`1 <= i <= n - 1`), at most one divider can be installed.

Divide the corridor into non-overlapping sections, where each section has **exactly two seats** with any number of plants. There may be multiple ways to perform the division. Two ways are **different** if there is a position with a room divider installed in the first way but not in the second way.

Return _the number of ways to divide the corridor_. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>. If there is no way, return `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/04/1.png)

**Input:** corridor = "SSPPSPS"

**Output:** 3

**Explanation:** There are 3 different ways to divide the corridor. 

The black bars in the above image indicate the two room dividers already installed. 

Note that in each of the ways, **each** section has exactly **two** seats.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/04/2.png)

**Input:** corridor = "PPSPSP"

**Output:** 1

**Explanation:** There is only 1 way to divide the corridor, by not installing any additional dividers. 

Installing any would create some section that does not have exactly two seats.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/12/12/3.png)

**Input:** corridor = "S"

**Output:** 0

**Explanation:** There is no way to divide the corridor because there will always be a section that does not have exactly two seats.

**Constraints:**

*   `n == corridor.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `corridor[i]` is either `'S'` or `'P'`.

## Solution

```kotlin
class Solution {
    fun numberOfWays(corridor: String): Int {
        var seat = 0
        val mod = 1e9.toInt() + 7
        for (i in 0 until corridor.length) {
            if (corridor[i] == 'S') {
                seat++
            }
        }
        if (seat == 0 || seat % 2 != 0) {
            return 0
        }
        seat /= 2
        var curr: Long = 0
        var ans: Long = 1
        var i = 0
        while (corridor[i] != 'S') {
            i++
        }
        i++
        while (seat > 1) {
            while (corridor[i] != 'S') {
                i++
            }
            i++
            while (corridor[i] != 'S') {
                i++
                curr++
            }
            curr++
            ans = ans * curr % mod
            curr = 0
            seat--
            i++
        }
        return ans.toInt()
    }
}
```