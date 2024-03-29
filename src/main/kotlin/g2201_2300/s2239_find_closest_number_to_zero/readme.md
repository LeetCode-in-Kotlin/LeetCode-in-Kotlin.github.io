[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2239\. Find Closest Number to Zero

Easy

Given an integer array `nums` of size `n`, return _the number with the value **closest** to_ `0` _in_ `nums`. If there are multiple answers, return _the number with the **largest** value_.

**Example 1:**

**Input:** nums = [-4,-2,1,4,8]

**Output:** 1

**Explanation:** 

The distance from -4 to 0 is \|-4\| = 4. 

The distance from -2 to 0 is \|-2\| = 2. 

The distance from 1 to 0 is \|1\| = 1. The distance from 4 to 0 is \|4\| = 4. 

The distance from 8 to 0 is \|8\| = 8. 

Thus, the closest number to 0 in the array is 1. 

**Example 2:**

**Input:** nums = [2,-1,1]

**Output:** 1

**Explanation:** 1 and -1 are both the closest numbers to 0, so 1 being larger is returned. 

**Constraints:**

*   `1 <= n <= 1000`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun findClosestNumber(nums: IntArray): Int {
        var mini = Int.MAX_VALUE
        var closestNum = 0
        for (n in nums) {
            if (mini > Math.abs(n)) {
                mini = Math.abs(n)
                closestNum = n
            } else if (mini == Math.abs(n) && closestNum < n) {
                closestNum = n
            }
        }
        return closestNum
    }
}
```