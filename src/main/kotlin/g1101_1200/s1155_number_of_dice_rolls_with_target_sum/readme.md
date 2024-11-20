[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1155\. Number of Dice Rolls With Target Sum

Medium

You have `n` dice and each die has `k` faces numbered from `1` to `k`.

Given three integers `n`, `k`, and `target`, return _the number of possible ways (out of the_ <code>k<sup>n</sup></code> _total ways)_ _to roll the dice so the sum of the face-up numbers equals_ `target`. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 1, k = 6, target = 3

**Output:** 1

**Explanation:** You throw one die with 6 faces. 

There is only one way to get a sum of 3.

**Example 2:**

**Input:** n = 2, k = 6, target = 7

**Output:** 6

**Explanation:** You throw two dice, each with 6 faces. 

There are 6 ways to get a sum of 7: 1+6, 2+5, 3+4, 4+3, 5+2, 6+1.

**Example 3:**

**Input:** n = 30, k = 30, target = 500

**Output:** 222616187

**Explanation:** The answer must be returned modulo 10<sup>9</sup> + 7.

**Constraints:**

*   `1 <= n, k <= 30`
*   `1 <= target <= 1000`

## Solution

```kotlin
class Solution {
    private var memo: Array<IntArray> = arrayOf()

    private var k = 0
    private fun dp(diceLeft: Int, targetLeft: Int): Int {
        if (diceLeft == 0) {
            return if (targetLeft == 0) {
                1
            } else {
                0
            }
        }
        if (memo[diceLeft][targetLeft] == -1) {
            var res = 0
            for (i in 1..Math.min(k, targetLeft)) {
                res += dp(diceLeft - 1, targetLeft - i)
                val modulo = 1000000007
                res %= modulo
            }
            memo[diceLeft][targetLeft] = res
        }
        return memo[diceLeft][targetLeft]
    }

    fun numRollsToTarget(n: Int, k: Int, target: Int): Int {
        this.k = k
        memo = Array(n + 1) { IntArray(target + 1) }
        for (i in memo) {
            i.fill(-1)
        }
        return dp(n, target)
    }
}
```