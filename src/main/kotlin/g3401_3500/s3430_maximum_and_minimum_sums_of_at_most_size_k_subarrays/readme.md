[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3430\. Maximum and Minimum Sums of at Most Size K Subarrays

Hard

You are given an integer array `nums` and a **positive** integer `k`. Return the sum of the **maximum** and **minimum** elements of all **non-empty subarrays** with **at most** `k` elements.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 20

**Explanation:**

The subarrays of `nums` with at most 2 elements are:

| **Subarray** | Minimum | Maximum | Sum |
|--------------|---------|---------|-----|
| `[1]`        | 1       | 1       | 2   |
| `[2]`        | 2       | 2       | 4   |
| `[3]`        | 3       | 3       | 6   |
| `[1, 2]`     | 1       | 2       | 3   |
| `[2, 3]`     | 2       | 3       | 5   |
| **Final Total** |         |         | 20  |

The output would be 20.

**Example 2:**

**Input:** nums = [1,-3,1], k = 2

**Output:** \-6

**Explanation:**

The subarrays of `nums` with at most 2 elements are:

| **Subarray**   | Minimum | Maximum | Sum  |
|----------------|---------|---------|------|
| `[1]`          | 1       | 1       | 2    |
| `[-3]`         | -3      | -3      | -6   |
| `[1]`          | 1       | 1       | 2    |
| `[1, -3]`      | -3      | 1       | -2   |
| `[-3, 1]`      | -3      | 1       | -2   |
| **Final Total**|         |         | -6   |

The output would be -6.

**Constraints:**

*   `1 <= nums.length <= 80000`
*   `1 <= k <= nums.length`
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minMaxSubarraySum(nums: IntArray, k: Int): Long {
        val sum = solve(nums, k)
        for (i in nums.indices) {
            nums[i] = -nums[i]
        }
        return sum - solve(nums, k)
    }

    private fun solve(nums: IntArray, k: Int): Long {
        val n = nums.size
        val left = IntArray(n)
        val right = IntArray(n)
        val st = IntArray(n)
        var top = -1
        for (i in 0..<n) {
            val num = nums[i]
            while (top != -1 && num < nums[st[top]]) {
                right[st[top--]] = i
            }
            left[i] = if (top == -1) -1 else st[top]
            st[++top] = i
        }
        while (0 <= top) {
            right[st[top--]] = n
        }
        var ans: Long = 0
        for (i in 0..<n) {
            val num = nums[i]
            val l = min((i - left[i]), k)
            val r = min((right[i] - i), k)
            if (l + r - 1 <= k) {
                ans += num.toLong() * l * r
            } else {
                val cnt = (k - r + 1).toLong() * r + (l + r - k - 1).toLong() * (r + k - l) / 2
                ans += num * cnt
            }
        }
        return ans
    }
}
```