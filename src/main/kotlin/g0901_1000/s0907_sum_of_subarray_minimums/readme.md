[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 907\. Sum of Subarray Minimums

Medium

Given an array of integers arr, find the sum of `min(b)`, where `b` ranges over every (contiguous) subarray of `arr`. Since the answer may be large, return the answer **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [3,1,2,4]

**Output:** 17

**Explanation:** 

Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 

Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1. 

Sum is 17.

**Example 2:**

**Input:** arr = [11,81,94,43,3]

**Output:** 444

**Constraints:**

*   <code>1 <= arr.length <= 3 * 10<sup>4</sup></code>
*   <code>1 <= arr[i] <= 3 * 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    private fun calculateRight(i: Int, start: Int, right: IntArray, arr: IntArray, len: Int): Int {
        if (start >= len) {
            return 0
        }
        return if (arr[start] < arr[i]) {
            0
        } else (1 + right[start] + calculateRight(i, start + right[start] + 1, right, arr, len)) % MOD
    }

    private fun calculateLeft(i: Int, start: Int, left: IntArray, arr: IntArray, len: Int): Int {
        if (start < 0) {
            return 0
        }
        return if (arr[start] <= arr[i]) {
            0
        } else (1 + left[start] + calculateLeft(i, start - left[start] - 1, left, arr, len)) % MOD
    }

    fun sumSubarrayMins(arr: IntArray): Int {
        val len = arr.size
        val right = IntArray(len)
        val left = IntArray(len)
        right[len - 1] = 0
        for (i in len - 2 downTo 0) {
            right[i] = calculateRight(i, i + 1, right, arr, len)
        }
        left[0] = 0
        for (i in 1 until len) {
            left[i] = calculateLeft(i, i - 1, left, arr, len)
        }
        var answer = 0
        for (i in 0 until len) {
            val model: Long = 1000000007
            answer += ((1 + left[i]) * (1 + right[i]).toLong() % model * arr[i] % model).toInt()
            answer %= MOD
        }
        return answer
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```