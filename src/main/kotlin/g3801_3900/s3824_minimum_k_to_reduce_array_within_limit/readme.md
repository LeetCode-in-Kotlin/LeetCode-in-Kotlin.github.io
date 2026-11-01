[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3824\. Minimum K to Reduce Array Within Limit

Medium

You are given a **positive** integer array `nums`.

For a positive integer `k`, define `nonPositive(nums, k)` as the **minimum** number of **operations** needed to make every element of `nums` **non-positive**. In one operation, you can choose an index `i` and reduce `nums[i]` by `k`.

Return an integer denoting the **minimum** value of `k` such that <code>nonPositive(nums, k) <= k<sup>2</sup></code>.

**Example 1:**

**Input:** nums = [3,7,5]

**Output:** 3

**Explanation:**

When `k = 3`, <code>nonPositive(nums, k) = 6 <= k<sup>2</sup></code>.

*   Reduce `nums[0] = 3` one time. `nums[0]` becomes `3 - 3 = 0`.
*   Reduce `nums[1] = 7` three times. `nums[1]` becomes `7 - 3 - 3 - 3 = -2`.
*   Reduce `nums[2] = 5` two times. `nums[2]` becomes `5 - 3 - 3 = -1`.

**Example 2:**

**Input:** nums = [1]

**Output:** 1

**Explanation:**

When `k = 1`, <code>nonPositive(nums, k) = 1 <= k<sup>2</sup></code>.

*   Reduce `nums[0] = 1` one time. `nums[0]` becomes `1 - 1 = 0`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.cbrt
import kotlin.math.ceil
import kotlin.math.max
import kotlin.math.sqrt

class Solution {
    fun minimumK(nums: IntArray): Int {
        val n = nums.size
        var mx = 0
        for (x in nums) {
            mx = max(mx, x)
        }
        var total: Long = 0
        for (x in nums) {
            total += x.toLong()
        }
        var left = max(ceil(sqrt(n.toDouble())).toInt(), ceil(cbrt(total.toDouble())).toInt()) - 1
        var right = ceil(sqrt(nonPositive(nums, left + 1).toDouble())).toInt()
        while (left + 1 < right) {
            val k = (left + right) / 2
            if (nonPositive(nums, k) <= k.toLong() * k) {
                right = k
            } else {
                left = k
            }
        }
        return left + 1
    }

    private fun nonPositive(nums: IntArray, k: Int): Long {
        var sum = nums.size.toLong()
        for (x in nums) {
            sum += ((x - 1) / k).toLong()
        }
        return sum
    }
}
```