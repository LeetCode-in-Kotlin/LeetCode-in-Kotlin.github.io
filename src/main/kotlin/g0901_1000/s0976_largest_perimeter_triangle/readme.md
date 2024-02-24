[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 976\. Largest Perimeter Triangle

Easy

Given an integer array `nums`, return _the largest perimeter of a triangle with a non-zero area, formed from three of these lengths_. If it is impossible to form any triangle of a non-zero area, return `0`.

**Example 1:**

**Input:** nums = [2,1,2]

**Output:** 5

**Explanation:** You can form a triangle with three side lengths: 1, 2, and 2.

**Example 2:**

**Input:** nums = [1,2,1,10]

**Output:** 0

**Explanation:** 

You cannot use the side lengths 1, 1, and 2 to form a triangle. 

You cannot use the side lengths 1, 1, and 10 to form a triangle. 

You cannot use the side lengths 1, 2, and 10 to form a triangle. 

As we cannot use any three side lengths to form a triangle of non-zero area, we return 0.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun largestPerimeter(nums: IntArray): Int {
        nums.sortDescending()
        for (i in nums.size - 1 downTo 2) {
            if (nums[i] < nums[i - 1] + nums[i - 2]) {
                return nums[i] + nums[i - 1] + nums[i - 2]
            }
        }
        return 0
    }
}
```
