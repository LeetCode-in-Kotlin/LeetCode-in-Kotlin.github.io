[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3950\. Exactly One Consecutive Set Bits Pair

Easy

You are given an integer `n`.

Return `true` if its binary representation contains **exactly one adjacent pair** of set bits, and `false` otherwise.

**Example 1:**

**Input:** n = 6

**Output:** true

**Explanation:**

*   Binary representation of 6 is `110`.
*   There is exactly one adjacent pair of set bits (`"11"`). Thus, the answer is `true`.

**Example 2:**

**Input:** n = 5

**Output:** false

**Explanation:**

*   Binary representation of 5 is `101`.
*   There is no adjacent pair of set bits. Thus, the answer is `false`.

**Constraints:**

*   <code>0 <= n <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun consecutiveSetBits(n: Int): Boolean {
        var n = n
        var cnt = 0
        while (n != 0) {
            if ((n and 3) == 3) {
                cnt++
            }
            n = n shr 1
        }
        return cnt == 1
    }
}
```