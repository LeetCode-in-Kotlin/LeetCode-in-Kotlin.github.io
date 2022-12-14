[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 390\. Elimination Game

Medium

You have a list `arr` of all integers in the range `[1, n]` sorted in a strictly increasing order. Apply the following algorithm on `arr`:

*   Starting from left to right, remove the first number and every other number afterward until you reach the end of the list.
*   Repeat the previous step again, but this time from right to left, remove the rightmost number and every other number from the remaining numbers.
*   Keep repeating the steps again, alternating left to right and right to left, until a single number remains.

Given the integer `n`, return _the last number that remains in_ `arr`.

**Example 1:**

**Input:** n = 9

**Output:** 6

**Explanation:**

    arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    arr = [2, 4, 6, 8]
    arr = [2, 6]
    arr = [6]

**Example 2:**

**Input:** n = 1

**Output:** 1

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun lastRemaining(n: Int): Int {
        return if (n == 1) 1 else 2 * (n / 2 - lastRemaining(n / 2) + 1)
    }
}
```