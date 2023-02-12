[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 611\. Valid Triangle Number

Medium

Given an integer array `nums`, return _the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle_.

**Example 1:**

**Input:** nums = [2,2,3,4]

**Output:** 3

**Explanation:** Valid combinations are: 

2,3,4 (using the first 2) 

2,3,4 (using the second 2) 

2,2,3

**Example 2:**

**Input:** nums = [4,2,3,4]

**Output:** 4

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `0 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun triangleNumber(nums: IntArray): Int {
        val n: Int
        var max = 0
        val count = IntArray(1001)
        for (i in nums) {
            count[i]++
            max = Math.max(max, i)
        }
        count[0] = 0
        var idx = 0
        for (i in 1..max) {
            var j = 0
            while (j < count[i]) {
                nums[idx] = i
                ++j
                ++idx
            }
            count[i] += count[i - 1]
        }
        n = idx
        var r = 0
        for (i in 0 until n - 2) {
            for (j in i + 1 until n - 1) {
                if (nums[i] + nums[j] > max) {
                    r += (n - j) * (n - j - 1) / 2
                    break
                }
                r += count[nums[i] + nums[j] - 1] - j - 1
            }
        }
        return r
    }
}
```