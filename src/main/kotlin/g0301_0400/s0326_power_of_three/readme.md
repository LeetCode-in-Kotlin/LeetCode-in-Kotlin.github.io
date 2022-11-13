[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 324\. Wiggle Sort II

Medium

Given an integer array `nums`, reorder it such that `nums[0] < nums[1] > nums[2] < nums[3]...`.

You may assume the input array always has a valid answer.

**Example 1:**

**Input:** nums = [1,5,1,1,6,4]

**Output:** [1,6,1,5,1,4]

**Explanation:** [1,4,1,5,1,6] is also accepted.

**Example 2:**

**Input:** nums = [1,3,2,2,3,1]

**Output:** [2,3,1,3,1,2]

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   `0 <= nums[i] <= 5000`
*   It is guaranteed that there will be an answer for the given input `nums`.

**Follow Up:** Can you do it in `O(n)` time and/or **in-place** with `O(1)` extra space?

## Solution

```kotlin
class Solution {
    fun isPowerOfThree(n: Int): Boolean {
        if (n == 1 || n == 3) {
            return true
        }
        if (n == 0 || n % 3 != 0) {
            return false
        }
        return isPowerOfThree(n / 3)
    }
}
```