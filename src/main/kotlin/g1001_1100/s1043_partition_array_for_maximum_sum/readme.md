[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1043\. Partition Array for Maximum Sum

Medium

Given an integer array `arr`, partition the array into (contiguous) subarrays of length **at most** `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return _the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a **32-bit** integer._

**Example 1:**

**Input:** arr = [1,15,7,9,2,5,10], k = 3

**Output:** 84

**Explanation:** arr becomes [15,15,15,9,10,10,10]

**Example 2:**

**Input:** arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4

**Output:** 83

**Example 3:**

**Input:** arr = [1], k = 1

**Output:** 1

**Constraints:**

*   `1 <= arr.length <= 500`
*   <code>0 <= arr[i] <= 10<sup>9</sup></code>
*   `1 <= k <= arr.length`

## Solution

```kotlin
class Solution {
    fun maxSumAfterPartitioning(arr: IntArray, k: Int): Int {
        val n = arr.size
        val dp = IntArray(n)
        for (right in 0 until n) {
            var localMax = arr[right]
            for (left in right downTo (-1).coerceAtLeast(right - k) + 1) {
                localMax = localMax.coerceAtLeast(arr[left])
                if (left == 0) {
                    dp[right] = dp[right].coerceAtLeast((right + 1) * localMax)
                } else {
                    dp[right] = dp[right].coerceAtLeast(dp[left - 1] + (right - left + 1) * localMax)
                }
            }
        }
        return dp[n - 1]
    }
}
```