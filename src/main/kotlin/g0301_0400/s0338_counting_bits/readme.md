[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 300\. Longest Increasing Subsequence

Medium

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Example 1:**

**Input:** nums = [10,9,2,5,3,7,101,18]

**Output:** 4

**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**

**Input:** nums = [0,1,0,3,2,3]

**Output:** 4

**Example 3:**

**Input:** nums = [7,7,7,7,7,7,7]

**Output:** 1

**Constraints:**

*   `1 <= nums.length <= 2500`
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

## Solution

```kotlin
class Solution {
    fun countBits(num: Int): IntArray {
        val result = IntArray(num + 1)
        var borderPos = 1
        var incrPos = 1
        for (i in 1 until result.size) {
            // when we reach pow of 2 ,  reset borderPos and incrPos
            if (incrPos == borderPos) {
                result[i] = 1
                incrPos = 1
                borderPos = i
            } else {
                result[i] = 1 + result[incrPos++]
            }
        }
        return result
    }
}
```