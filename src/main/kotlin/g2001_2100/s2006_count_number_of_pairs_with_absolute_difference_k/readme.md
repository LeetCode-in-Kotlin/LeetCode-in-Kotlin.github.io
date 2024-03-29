[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2006\. Count Number of Pairs With Absolute Difference K

Easy

Given an integer array `nums` and an integer `k`, return _the number of pairs_ `(i, j)` _where_ `i < j` _such that_ `|nums[i] - nums[j]| == k`.

The value of `|x|` is defined as:

*   `x` if `x >= 0`.
*   `-x` if `x < 0`.

**Example 1:**

**Input:** nums = [1,2,2,1], k = 1

**Output:** 4

**Explanation:** The pairs with an absolute difference of 1 are: 

- \[**1**,**2**,2,1] 

- \[**1**,2,**2**,1] 

- \[1,**2**,2,**1**] 

- \[1,2,**2**,**1**]

**Example 2:**

**Input:** nums = [1,3], k = 3

**Output:** 0

**Explanation:** There are no pairs with an absolute difference of 3.

**Example 3:**

**Input:** nums = [3,2,1,5,4], k = 2

**Output:** 3

**Explanation:** The pairs with an absolute difference of 2 are: 

- \[**3**,2,**1**,5,4] 

- \[**3**,2,1,**5**,4] 

- \[3,**2**,1,5,**4**]

**Constraints:**

*   `1 <= nums.length <= 200`
*   `1 <= nums[i] <= 100`
*   `1 <= k <= 99`

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun countKDifference(nums: IntArray, k: Int): Int {
        var pairs = 0
        for (i in 0 until nums.size - 1) {
            for (j in i + 1 until nums.size) {
                if (abs(nums[i] - nums[j]) == k) {
                    pairs++
                }
            }
        }
        return pairs
    }
}
```