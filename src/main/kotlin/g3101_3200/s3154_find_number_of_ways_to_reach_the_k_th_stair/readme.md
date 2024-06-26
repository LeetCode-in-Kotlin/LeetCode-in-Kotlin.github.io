[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3154\. Find Number of Ways to Reach the K-th Stair

Hard

You are given a **non-negative** integer `k`. There exists a staircase with an infinite number of stairs, with the **lowest** stair numbered 0.

Alice has an integer `jump`, with an initial value of 0. She starts on stair 1 and wants to reach stair `k` using **any** number of **operations**. If she is on stair `i`, in one **operation** she can:

*   Go down to stair `i - 1`. This operation **cannot** be used consecutively or on stair 0.
*   Go up to stair <code>i + 2<sup>jump</sup></code>. And then, `jump` becomes `jump + 1`.

Return the _total_ number of ways Alice can reach stair `k`.

**Note** that it is possible that Alice reaches the stair `k`, and performs some operations to reach the stair `k` again.

**Example 1:**

**Input:** k = 0

**Output:** 2

**Explanation:**

The 2 possible ways of reaching stair 0 are:

*   Alice starts at stair 1.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 0.
*   Alice starts at stair 1.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 0.
    *   Using an operation of the second type, she goes up 2<sup>0</sup> stairs to reach stair 1.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 0.

**Example 2:**

**Input:** k = 1

**Output:** 4

**Explanation:**

The 4 possible ways of reaching stair 1 are:

*   Alice starts at stair 1. Alice is at stair 1.
*   Alice starts at stair 1.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 0.
    *   Using an operation of the second type, she goes up 2<sup>0</sup> stairs to reach stair 1.
*   Alice starts at stair 1.
    *   Using an operation of the second type, she goes up 2<sup>0</sup> stairs to reach stair 2.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 1.
*   Alice starts at stair 1.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 0.
    *   Using an operation of the second type, she goes up 2<sup>0</sup> stairs to reach stair 1.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 0.
    *   Using an operation of the second type, she goes up 2<sup>1</sup> stairs to reach stair 2.
    *   Using an operation of the first type, she goes down 1 stair to reach stair 1.

**Constraints:**

*   <code>0 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun waysToReachStair(k: Int): Int {
        var x = 1
        var y = 1
        var a = 0
        while (x > 0 && x - y <= k) {
            if (x >= k) {
                a += combi(y, x - k)
            }
            x = x shl 1
            y++
        }
        return a
    }

    private fun combi(a: Int, b: Int): Int {
        var b = b
        if (b > a - b) {
            b = a - b
        }
        var r: Long = 1
        for (i in 0 until b) {
            r *= (a - i).toLong()
            r /= (i + 1).toLong()
        }
        return r.toInt()
    }
}
```