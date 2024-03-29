[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1343\. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold

Medium

Given an array of integers `arr` and two integers `k` and `threshold`, return _the number of sub-arrays of size_ `k` _and average greater than or equal to_ `threshold`.

**Example 1:**

**Input:** arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4

**Output:** 3

**Explanation:** Sub-arrays [2,5,5],[5,5,5] and [5,5,8] have averages 4, 5 and 6 respectively. All other sub-arrays of size 3 have averages less than 4 (the threshold).

**Example 2:**

**Input:** arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5

**Output:** 6

**Explanation:** The first 6 sub-arrays of size 3 have averages greater than 5. Note that averages are not integers.

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>5</sup></code>
*   <code>1 <= arr[i] <= 10<sup>4</sup></code>
*   `1 <= k <= arr.length`
*   <code>0 <= threshold <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun numOfSubarrays(arr: IntArray, k: Int, threshold: Int): Int {
        var sum = 0
        for (i in 0 until k - 1) {
            sum += arr[i]
        }
        var count = 0
        for (i in k - 1 until arr.size) {
            sum += arr[i]
            if (i - k >= 0) {
                sum -= arr[i - k]
            }
            if (sum / k >= threshold) {
                count++
            }
        }
        return count
    }
}
```