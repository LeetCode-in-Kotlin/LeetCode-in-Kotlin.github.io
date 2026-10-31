[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3780\. Maximum Sum of Three Numbers Divisible by Three

Medium

You are given an integer array `nums`.

Your task is to choose **exactly three** integers from `nums` such that their sum is divisible by three.

Return the **maximum** possible sum of such a triplet. If no such triplet exists, return 0.

**Example 1:**

**Input:** nums = [4,2,3,1]

**Output:** 9

**Explanation:**

The valid triplets whose sum is divisible by 3 are:

*   `(4, 2, 3)` with a sum of `4 + 2 + 3 = 9`.
*   `(2, 3, 1)` with a sum of `2 + 3 + 1 = 6`.

Thus, the answer is 9.

**Example 2:**

**Input:** nums = [2,1,5]

**Output:** 0

**Explanation:**

No triplet forms a sum divisible by 3, so the answer is 0.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumSum(nums: IntArray): Int {
        val group: Array<IntArray> = Array<IntArray>(3) { IntArray(3) }
        for (num in nums) {
            val m = num % 3
            var max1 = group[m][0]
            var max2 = group[m][1]
            var max3 = group[m][2]
            if (num >= max1) {
                max3 = max2
                max2 = max1
                max1 = num
            } else if (num >= max2) {
                max3 = max2
                max2 = num
            } else if (num > max3) {
                max3 = num
            }
            group[m][0] = max1
            group[m][1] = max2
            group[m][2] = max3
        }
        var res = 0
        for (g in group) {
            var sum = 0
            var i = 0
            while (i < 3) {
                if (g[i] == 0) {
                    sum = 0
                    break
                }
                sum += g[i]
                i += 1
            }
            res = max(res, sum)
        }
        var max = 0
        for (g in group) {
            if (g[0] == 0) {
                max = 0
                break
            }
            max += g[0]
        }
        res = max(res, max)
        return res
    }
}
```