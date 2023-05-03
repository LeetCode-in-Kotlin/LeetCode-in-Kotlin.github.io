[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 935\. Knight Dialer

Medium

The chess knight has a **unique movement**, it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an **L**). The possible movements of chess knight are shown in this diagaram:

A chess knight can move as indicated in the chess diagram below:

![](https://assets.leetcode.com/uploads/2020/08/18/chess.jpg)

We have a chess knight and a phone pad as shown below, the knight **can only stand on a numeric cell** (i.e. blue cell).

![](https://assets.leetcode.com/uploads/2020/08/18/phone.jpg)

Given an integer `n`, return how many distinct phone numbers of length `n` we can dial.

You are allowed to place the knight **on any numeric cell** initially and then you should perform `n - 1` jumps to dial a number of length `n`. All jumps should be **valid** knight jumps.

As the answer may be very large, **return the answer modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 1

**Output:** 10

**Explanation:** We need to dial a number of length 1, so placing the knight over any numeric cell of the 10 cells is sufficient.

**Example 2:**

**Input:** n = 2

**Output:** 20

**Explanation:** All the valid number we can dial are [04, 06, 16, 18, 27, 29, 34, 38, 40, 43, 49, 60, 61, 67, 72, 76, 81, 83, 92, 94]

**Example 3:**

**Input:** n = 3131

**Output:** 136006598

**Explanation:** Please take care of the mod.

**Constraints:**

*   `1 <= n <= 5000`

## Solution

```kotlin
class Solution {
    fun knightDialer(n: Int): Int {
        if (n == 1) {
            return 10
        }
        val mod = 1000000007
        while (MEMO.size < n) {
            val cur = MEMO[MEMO.size - 1]
            val next = IntArray(10)
            for (i in 0..9) {
                for (d in MAP[i]!!) {
                    next[d] = (next[d] + cur[i]) % mod
                }
            }
            MEMO.add(next)
        }
        var sum = 0
        for (x in MEMO[n - 1]) {
            sum = (sum + x) % mod
        }
        return sum
    }

    companion object {
        private val MAP = arrayOfNulls<IntArray>(10)
        private val MEMO: MutableList<IntArray> = ArrayList()

        init {
            MAP[0] = intArrayOf(4, 6)
            MAP[1] = intArrayOf(6, 8)
            MAP[2] = intArrayOf(7, 9)
            MAP[3] = intArrayOf(4, 8)
            MAP[4] = intArrayOf(3, 9, 0)
            MAP[5] = IntArray(0)
            MAP[6] = intArrayOf(1, 7, 0)
            MAP[7] = intArrayOf(2, 6)
            MAP[8] = intArrayOf(1, 3)
            MAP[9] = intArrayOf(2, 4)
            MEMO.add(intArrayOf(1, 1, 1, 1, 1, 0, 1, 1, 1, 1))
        }
    }
}
```