[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2400\. Number of Ways to Reach a Position After Exactly k Steps

Medium

You are given two **positive** integers `startPos` and `endPos`. Initially, you are standing at position `startPos` on an **infinite** number line. With one step, you can move either one position to the left, or one position to the right.

Given a positive integer `k`, return _the number of **different** ways to reach the position_ `endPos` _starting from_ `startPos`_, such that you perform **exactly**_ `k` _steps_. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

Two ways are considered different if the order of the steps made is not exactly the same.

**Note** that the number line includes negative integers.

**Example 1:**

**Input:** startPos = 1, endPos = 2, k = 3

**Output:** 3

**Explanation:** We can reach position 2 from 1 in exactly 3 steps in three ways:

- 1 -> 2 -> 3 -> 2.

- 1 -> 2 -> 1 -> 2.

- 1 -> 0 -> 1 -> 2.

It can be proven that no other way is possible, so we return 3.

**Example 2:**

**Input:** startPos = 2, endPos = 5, k = 10

**Output:** 0

**Explanation:** It is impossible to reach position 5 from position 2 in exactly 10 steps. 

**Constraints:**

*   `1 <= startPos, endPos, k <= 1000`

## Solution

```kotlin
class Solution {
    private val mod = 1000000007

    fun numberOfWays(startPos: Int, endPos: Int, k: Int): Int {
        if (Math.abs(endPos - startPos) > k) {
            return 0
        }
        if (Math.abs(endPos - startPos + k) % 2 != 0) {
            return 0
        }
        val t = endPos - startPos
        val right = (k + t) / 2
        val min = Math.min(right, k - right)
        if (min == 0) {
            return 1
        }
        val rev = IntArray(min + 1)
        rev[1] = 1
        var ans = k
        for (i in 2..min) {
            rev[i] = ((mod - mod / i).toLong() * rev[mod % i].toLong() % mod).toInt()
            ans = (ans.toLong() * (k - i + 1).toLong() % mod).toInt()
            ans = (ans.toLong() * rev[i].toLong() % mod).toInt()
        }
        return ans
    }
}
```