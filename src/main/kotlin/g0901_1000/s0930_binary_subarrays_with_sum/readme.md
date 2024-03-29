[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 930\. Binary Subarrays With Sum

Medium

Given a binary array `nums` and an integer `goal`, return _the number of non-empty **subarrays** with a sum_ `goal`.

A **subarray** is a contiguous part of the array.

**Example 1:**

**Input:** nums = [1,0,1,0,1], goal = 2

**Output:** 4

**Explanation:** The 4 subarrays are bolded and underlined below: 

[<ins>**1,0,1**</ins>,0,1]

[<ins>**1,0,1,0**</ins>,1]

[1,<ins>**0,1,0,1**</ins>] 

[1,0,<ins>**1,0,1**</ins>]

**Example 2:**

**Input:** nums = [0,0,0,0,0], goal = 0

**Output:** 15

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   `nums[i]` is either `0` or `1`.
*   `0 <= goal <= nums.length`

## Solution

```kotlin
class Solution {
    fun numSubarraysWithSum(nums: IntArray, goal: Int): Int {
        return atmostK(nums, goal) - atmostK(nums, goal - 1)
    }

    fun atmostK(arr: IntArray, k: Int): Int {
        var cnt = 0
        var i = 0
        var j = 0
        var sum = 0
        while (j < arr.size) {
            sum += arr[j]
            if (sum > k) {
                while (i <= j && sum > k) {
                    sum -= arr[i]
                    i++
                }
            }
            cnt += j - i + 1
            j++
        }
        return cnt
    }
}
```