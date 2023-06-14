[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1493\. Longest Subarray of 1's After Deleting One Element

Medium

Given a binary array `nums`, you should delete one element from it.

Return _the size of the longest non-empty subarray containing only_ `1`_'s in the resulting array_. Return `0` if there is no such subarray.

**Example 1:**

**Input:** nums = [1,1,0,1]

**Output:** 3

**Explanation:** After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.

**Example 2:**

**Input:** nums = [0,1,1,1,0,1,1,0,1]

**Output:** 5

**Explanation:** After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].

**Example 3:**

**Input:** nums = [1,1,1]

**Output:** 2

**Explanation:** You must delete one element.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun longestSubarray(nums: IntArray): Int {
        var s = 0
        var e = 0
        var max = Int.MIN_VALUE
        var extraZero = false
        var allOne = true
        while (e < nums.size) {
            if (nums[e] == 1) {
                e++
            } else if (!extraZero) {
                allOne = false
                extraZero = true
                e++
            } else {
                allOne = false
                max = Math.max(max, e - s - 1)
                while (nums[s] != 0) {
                    s++
                }
                s++
                extraZero = false
            }
        }
        if (nums[e - 1] == 1) {
            max = Math.max(max, e - s - 1)
        }
        if (allOne) {
            return nums.size - 1
        }
        return if (max == Int.MIN_VALUE) 0 else max
    }
}
```