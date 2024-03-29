[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1848\. Minimum Distance to the Target Element

Easy

Given an integer array `nums` **(0-indexed)** and two integers `target` and `start`, find an index `i` such that `nums[i] == target` and `abs(i - start)` is **minimized**. Note that `abs(x)` is the absolute value of `x`.

Return `abs(i - start)`.

It is **guaranteed** that `target` exists in `nums`.

**Example 1:**

**Input:** nums = [1,2,3,4,5], target = 5, start = 3

**Output:** 1

**Explanation:** nums[4] = 5 is the only value equal to target, so the answer is abs(4 - 3) = 1.

**Example 2:**

**Input:** nums = [1], target = 1, start = 0

**Output:** 0

**Explanation:** nums[0] = 1 is the only value equal to target, so the answer is abs(0 - 0) = 0.

**Example 3:**

**Input:** nums = [1,1,1,1,1,1,1,1,1,1], target = 1, start = 0

**Output:** 0

**Explanation:** Every value of nums is 1, but nums[0] minimizes abs(i - start), which is abs(0 - 0) = 0.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>
*   `0 <= start < nums.length`
*   `target` is in `nums`.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun getMinDistance(nums: IntArray, target: Int, start: Int): Int {
        var result = 0
        var minDiff = Int.MAX_VALUE
        for (i in nums.indices) {
            if (nums[i] == target && abs(start - i) < minDiff) {
                minDiff = abs(start - i)
                result = minDiff
            }
        }
        return result
    }
}
```