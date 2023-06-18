[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1798\. Maximum Number of Consecutive Values You Can Make

Medium

You are given an integer array `coins` of length `n` which represents the `n` coins that you own. The value of the <code>i<sup>th</sup></code> coin is `coins[i]`. You can **make** some value `x` if you can choose some of your `n` coins such that their values sum up to `x`.

Return the _maximum number of consecutive integer values that you **can** **make** with your coins **starting** from and **including**_ `0`.

Note that you may have multiple coins of the same value.

**Example 1:**

**Input:** coins = [1,3]

**Output:** 2

**Explanation:** You can make the following values:

- 0: take []

- 1: take [1]

You can make 2 consecutive integer values starting from 0.

**Example 2:**

**Input:** coins = [1,1,1,4]

**Output:** 8

**Explanation:** You can make the following values:

- 0: take []

- 1: take [1]

- 2: take [1,1]

- 3: take [1,1,1]

- 4: take [4]

- 5: take [4,1]

- 6: take [4,1,1]

- 7: take [4,1,1,1]

You can make 8 consecutive integer values starting from 0.

**Example 3:**

**Input:** nums = [1,4,10,3,1]

**Output:** 20

**Constraints:**

*   `coins.length == n`
*   <code>1 <= n <= 4 * 10<sup>4</sup></code>
*   <code>1 <= coins[i] <= 4 * 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun getMaximumConsecutive(coins: IntArray): Int {
        val count = IntArray(40001)
        for (c in coins) {
            count[c]++
        }
        var res = 1
        var i = 1
        while (i < count.size && i <= res) {
            res += i * count[i]
            i++
        }
        return res
    }
}
```