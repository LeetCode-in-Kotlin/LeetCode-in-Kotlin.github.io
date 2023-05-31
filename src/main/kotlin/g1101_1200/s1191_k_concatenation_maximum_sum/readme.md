[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1191\. K-Concatenation Maximum Sum

Medium

Given an integer array `arr` and an integer `k`, modify the array by repeating it `k` times.

For example, if `arr = [1, 2]` and `k = 3` then the modified array will be `[1, 2, 1, 2, 1, 2]`.

Return the maximum sub-array sum in the modified array. Note that the length of the sub-array can be `0` and its sum in that case is `0`.

As the answer can be very large, return the answer **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [1,2], k = 3

**Output:** 9

**Example 2:**

**Input:** arr = [1,-2,1], k = 5

**Output:** 2

**Example 3:**

**Input:** arr = [-1,-2], k = 7

**Output:** 0

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= arr[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    private var mod = (1e9 + 7).toInt()

    fun kConcatenationMaxSum(arr: IntArray, k: Int): Int {
        var sum: Long = 0
        for (i in arr.indices) {
            sum += arr[i].toLong()
        }
        return if (sum <= 0 || k == 1) {
            var cb: Long = 0
            var ob: Long = 0
            for (i in arr.indices) {
                cb = if (arr[i] + cb > arr[i]) {
                    arr[i] + cb
                } else {
                    arr[i].toLong()
                }
                if (ob < cb) {
                    ob = cb
                }
            }
            if (k == 1) {
                return (ob % mod).toInt()
            }
            for (i in arr.indices) {
                cb = if (arr[i] + cb > arr[i]) {
                    arr[i] + cb
                } else {
                    arr[i].toLong()
                }
                if (ob < cb) {
                    ob = cb
                }
            }
            (ob % mod).toInt()
        } else {
            var max1: Long = 0
            var smax: Long = 0
            for (i in arr.indices.reversed()) {
                smax += arr[i].toLong()
                if (smax > max1) {
                    max1 = smax
                }
            }
            max1 %= mod.toLong()
            var max2: Long = 0
            smax = 0
            for (i in arr.indices) {
                smax += arr[i].toLong()
                if (smax > max2) {
                    max2 = smax
                }
            }
            max2 %= mod.toLong()
            val ans = max1 + (k - 2) * sum + max2
            (ans % mod).toInt()
        }
    }
}
```