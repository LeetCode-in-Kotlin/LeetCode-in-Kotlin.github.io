[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 795\. Number of Subarrays with Bounded Maximum

Medium

Given an integer array `nums` and two integers `left` and `right`, return _the number of contiguous non-empty **subarrays** such that the value of the maximum array element in that subarray is in the range_ `[left, right]`.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [2,1,4,3], left = 2, right = 3

**Output:** 3

**Explanation:** There are three subarrays that meet the requirements: [2], [2, 1], [3].

**Example 2:**

**Input:** nums = [2,9,2,5,6], left = 2, right = 8

**Output:** 7

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= left <= right <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun numSubarrayBoundedMax(arr: IntArray, left: Int, right: Int): Int {
        var ans = 0
        var si = 0
        var ei = 0
        var prev = 0
        while (ei < arr.size) {
            // if in range
            if (arr[ei] in left..right) {
                prev = ei - si + 1
                ans += prev
            } else if (arr[ei] < left) {
                ans += prev
            } else if (arr[ei] > right) {
                prev = 0
                si = ei + 1
            }
            ei++
        }
        return ans
    }
}
```