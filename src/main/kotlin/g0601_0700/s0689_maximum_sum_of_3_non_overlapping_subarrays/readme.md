[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 689\. Maximum Sum of 3 Non-Overlapping Subarrays

Hard

Given an integer array `nums` and an integer `k`, find three non-overlapping subarrays of length `k` with maximum sum and return them.

Return the result as a list of indices representing the starting position of each interval (**0-indexed**). If there are multiple answers, return the lexicographically smallest one.

**Example 1:**

**Input:** nums = [1,2,1,2,6,7,5,1], k = 2

**Output:** [0,3,5]

**Explanation:**

Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].

We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger. 

**Example 2:**

**Input:** nums = [1,2,1,2,1,2,1,2,1], k = 2

**Output:** [0,2,4] 

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] < 2<sup>16</sup></code>
*   `1 <= k <= floor(nums.length / 3)`

## Solution

```kotlin
class Solution {
    fun maxSumOfThreeSubarrays(nums: IntArray, k: Int): IntArray {
        val len = nums.size
        if (len < 3 * k) {
            return intArrayOf()
        }
        val res = IntArray(3)
        val left = Array(2) { IntArray(len) }
        val right = Array(2) { IntArray(len) }
        var s = 0
        for (i in 0 until k) {
            s += nums[i]
        }
        left[0][k - 1] = s
        run {
            var i = k
            while (i + 2 * k <= len) {
                s = s + nums[i] - nums[i - k]
                if (s > left[0][i - 1]) {
                    left[0][i] = s
                    left[1][i] = i - k + 1
                } else {
                    left[0][i] = left[0][i - 1]
                    left[1][i] = left[1][i - 1]
                }
                i++
            }
        }
        s = 0
        for (i in len - 1 downTo len - k) {
            s += nums[i]
        }
        right[0][len - k] = s
        right[1][len - k] = len - k
        for (i in len - k - 1 downTo 0) {
            s = s + nums[i] - nums[i + k]
            if (s >= right[0][i + 1]) {
                right[0][i] = s
                right[1][i] = i
            } else {
                right[0][i] = right[0][i + 1]
                right[1][i] = right[1][i + 1]
            }
        }
        var mid = 0
        for (i in k until 2 * k) {
            mid += nums[i]
        }
        var max = 0
        var i = k
        while (i + 2 * k <= len) {
            val total = left[0][i - 1] + right[0][i + k] + mid
            if (total > max) {
                res[0] = left[1][i - 1]
                res[1] = i
                res[2] = right[1][i + k]
                max = total
            }
            mid = mid + nums[i + k] - nums[i]
            i++
        }
        return res
    }
}
```