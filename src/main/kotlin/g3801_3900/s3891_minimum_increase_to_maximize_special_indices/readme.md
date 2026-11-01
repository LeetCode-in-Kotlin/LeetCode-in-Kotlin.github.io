[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3891\. Minimum Increase to Maximize Special Indices

Medium

You are given an integer array `nums` of length `n`.

An index `i` (`0 < i < n - 1`) is **special** if `nums[i] > nums[i - 1]` and `nums[i] > nums[i + 1]`.

You may perform operations where you choose **any** index `i` and **increase** `nums[i]` by 1.

Your goal is to:

*   **Maximize** the number of **special** indices.
*   **Minimize** the total number of **operations** required to achieve that **maximum**.

Return an integer denoting the **minimum** total number of operations required.

**Example 1:**

**Input:** nums = [1,2,2]

**Output:** 1

**Explanation:**

*   Start with `nums = [1, 2, 2]`.
*   Increase `nums[1]` by 1, array becomes `[1, 3, 2]`.
*   The final array is `[1, 3, 2]` has 1 special index, which is the maximum achievable.
*   It is impossible to achieve this number of special indices with fewer operations. Thus, the answer is 1.

**Example 2:**

**Input:** nums = [2,1,1,3]

**Output:** 2

**Explanation:**

*   Start with `nums = [2, 1, 1, 3]`.
*   Perform 2 operations at index 1, array becomes `[2, 3, 1, 3]`.
*   The final array is `[2, 3, 1, 3]` has 1 special index, which is the maximum achievable. Thus, the answer is 2.

**Example 3:**

**Input:** nums = [5,2,1,4,3]

**Output:** 4

**Explanation:**

*   Start with `nums = [5, 2, 1, 4, 3]`.
*   Perform 4 operations at index 1, array becomes `[5, 6, 1, 4, 3]`.
*   The final array is `[5, 6, 1, 4, 3]` has 2 special indices, which is the maximum achievable. Thus, the answer is 4.

**Constraints:**

*   <code>3 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minIncrease(nums: IntArray): Long {
        val n = nums.size
        var min: Long = 0
        if ((n and 1) == 1) {
            var i = 1
            while (i < n) {
                min += max(max(nums[i - 1], nums[i + 1]) + 1 - nums[i], 0).toLong()
                i += 2
            }
        } else {
            val starting = longArrayOf(0, 0)
            var i = 1
            while (i < n - 1) {
                val firstOp = max(max(nums[i - 1], nums[i + 1]) + 1 - nums[i], 0)
                val secondOp = max(max(nums[i], nums[i + 2]) + 1 - nums[i + 1], 0)
                starting[1] = min(starting[0], starting[1]) + secondOp
                starting[0] += firstOp.toLong()
                i += 2
            }
            min = min(starting[0], starting[1])
        }
        return min
    }
}
```