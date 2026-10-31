[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3741\. Minimum Distance Between Three Equal Elements II

Medium

You are given an integer array `nums`.

A tuple `(i, j, k)` of 3 **distinct** indices is **good** if `nums[i] == nums[j] == nums[k]`.

The **distance** of a **good** tuple is `abs(i - j) + abs(j - k) + abs(k - i)`, where `abs(x)` denotes the **absolute value** of `x`.

Return an integer denoting the **minimum** possible **distance** of a **good** tuple. If no **good** tuples exist, return `-1`.

**Example 1:**

**Input:** nums = [1,2,1,1,3]

**Output:** 6

**Explanation:**

The minimum distance is achieved by the good tuple `(0, 2, 3)`.

`(0, 2, 3)` is a good tuple because `nums[0] == nums[2] == nums[3] == 1`. Its distance is `abs(0 - 2) + abs(2 - 3) + abs(3 - 0) = 2 + 1 + 3 = 6`.

**Example 2:**

**Input:** nums = [1,1,2,3,2,1,2]

**Output:** 8

**Explanation:**

The minimum distance is achieved by the good tuple `(2, 4, 6)`.

`(2, 4, 6)` is a good tuple because `nums[2] == nums[4] == nums[6] == 2`. Its distance is `abs(2 - 4) + abs(4 - 6) + abs(6 - 2) = 2 + 2 + 4 = 8`.

**Example 3:**

**Input:** nums = [1]

**Output:** \-1

**Explanation:**

There are no good tuples. Therefore, the answer is -1.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   `1 <= nums[i] <= n`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimumDistance(nums: IntArray): Int {
        val n = nums.size
        var ans = Int.MAX_VALUE
        val prev1 = IntArray(n + 1)
        val prev2 = IntArray(n + 1)
        for (i in 0..<n + 1) {
            prev2[i] = -1
            prev1[i] = prev2[i]
        }
        for (i in 0..<n) {
            val value = nums[i]
            if (prev2[value] != -1) {
                ans = min(ans, (i - prev2[value]))
            }
            prev2[value] = prev1[value]
            prev1[value] = i
        }
        if (ans < 100002) {
            return ans * 2
        }
        return -1
    }
}
```