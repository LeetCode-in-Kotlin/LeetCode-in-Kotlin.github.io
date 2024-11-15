[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3346\. Maximum Frequency of an Element After Performing Operations I

Medium

You are given an integer array `nums` and two integers `k` and `numOperations`.

You must perform an **operation** `numOperations` times on `nums`, where in each operation you:

*   Select an index `i` that was **not** selected in any previous operations.
*   Add an integer in the range `[-k, k]` to `nums[i]`.

Return the **maximum** possible frequency of any element in `nums` after performing the **operations**.

**Example 1:**

**Input:** nums = [1,4,5], k = 1, numOperations = 2

**Output:** 2

**Explanation:**

We can achieve a maximum frequency of two by:

*   Adding 0 to `nums[1]`. `nums` becomes `[1, 4, 5]`.
*   Adding -1 to `nums[2]`. `nums` becomes `[1, 4, 4]`.

**Example 2:**

**Input:** nums = [5,11,20,20], k = 5, numOperations = 1

**Output:** 2

**Explanation:**

We can achieve a maximum frequency of two by:

*   Adding 0 to `nums[1]`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>0 <= k <= 10<sup>5</sup></code>
*   `0 <= numOperations <= nums.length`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    private fun getMax(nums: IntArray): Int {
        var max = nums[0]
        for (num in nums) {
            max = max(num, max)
        }
        return max
    }

    fun maxFrequency(nums: IntArray, k: Int, numOperations: Int): Int {
        val maxNum = getMax(nums)
        val n = maxNum + k + 2
        val freq = IntArray(n)
        for (num in nums) {
            freq[num]++
        }
        val pref = IntArray(n)
        pref[0] = freq[0]
        for (i in 1 until n) {
            pref[i] = pref[i - 1] + freq[i]
        }
        var res = 0
        for (i in 0 until n) {
            val left: Int = max(0, (i - k))
            val right: Int = min((n - 1), (i + k))
            var tot = pref[right]
            if (left > 0) {
                tot -= pref[left - 1]
            }
            res = max(res, (freq[i] + min(numOperations, (tot - freq[i]))))
        }
        return res
    }
}
```