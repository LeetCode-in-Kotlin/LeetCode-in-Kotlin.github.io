[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3795\. Minimum Subarray Length With Distinct Sum At Least K

Medium

You are given an integer array `nums` and an integer `k`.

Return the **minimum** length of a **non-empty subarrays** whose sum of the **distinct** values present in that subarray (each value counted once) is **at least** `k`. If no such subarray exists, return -1.

**Example 1:**

**Input:** nums = [2,2,3,1], k = 4

**Output:** 2

**Explanation:**

The subarray `[2, 3]` has distinct elements `{2, 3}` whose sum is `2 + 3 = 5`, which is at least `k = 4`. Thus, the answer is 2.

**Example 2:**

**Input:** nums = [3,2,3,4], k = 5

**Output:** 2

**Explanation:**

The subarray `[3, 2]` has distinct elements `{3, 2}` whose sum is `3 + 2 = 5`, which is at least `k = 5`. Thus, the answer is 2.

**Example 3:**

**Input:** nums = [5,5,4], k = 5

**Output:** 1

**Explanation:**

The subarray `[5]` has distinct elements `{5}` whose sum is `5`, which is at least `k = 5`. Thus, the answer is 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minLength(nums: IntArray, k: Int): Int {
        for (i in nums) {
            if (i >= k) {
                return 1
            }
        }
        var ans = Int.MAX_VALUE
        val alt = -1
        var i = 0
        var j = 1
        var sum = nums[0]
        if (sum >= k) {
            return 1
        }
        val hm: MutableMap<Int?, Int?> = HashMap<Int?, Int?>()
        hm[nums[0]] = 1
        while (i < nums.size && j < nums.size) {
            hm[nums[j]] = hm.getOrDefault(nums[j], 0)!! + 1
            if (hm[nums[j]] == 1) {
                sum += nums[j]
            }
            while (sum >= k) {
                ans = min(ans, j - i + 1)
                hm[nums[i]] = hm[nums[i]]!! - 1
                if (hm[nums[i]] == 0) {
                    sum -= nums[i]
                }
                i++
            }
            j++
        }
        return if (ans != Int.MAX_VALUE) ans else alt
    }
}
```