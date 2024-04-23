[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3101\. Count Alternating Subarrays

Medium

You are given a binary array `nums`.

We call a subarray **alternating** if **no** two **adjacent** elements in the subarray have the **same** value.

Return _the number of alternating subarrays in_ `nums`.

**Example 1:**

**Input:** nums = [0,1,1,1]

**Output:** 5

**Explanation:**

The following subarrays are alternating: `[0]`, `[1]`, `[1]`, `[1]`, and `[0,1]`.

**Example 2:**

**Input:** nums = [1,0,1,0]

**Output:** 10

**Explanation:**

Every subarray of the array is alternating. There are 10 possible subarrays that we can choose.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun countAlternatingSubarrays(nums: IntArray): Long {
        var count: Long = 0
        var length: Long
        var start = 0
        var end = 1
        while (end < nums.size) {
            if (nums[end] != nums[end - 1]) {
                end++
            } else {
                length = end - start.toLong()
                count += (length * (length + 1)) / 2
                start = end
                end++
            }
        }
        length = end - start.toLong()
        count += (length * (length + 1)) / 2
        return count
    }
}
```