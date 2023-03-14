[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 775\. Global and Local Inversions

Medium

You are given an integer array `nums` of length `n` which represents a permutation of all the integers in the range `[0, n - 1]`.

The number of **global inversions** is the number of the different pairs `(i, j)` where:

*   `0 <= i < j < n`
*   `nums[i] > nums[j]`

The number of **local inversions** is the number of indices `i` where:

*   `0 <= i < n - 1`
*   `nums[i] > nums[i + 1]`

Return `true` _if the number of **global inversions** is equal to the number of **local inversions**_.

**Example 1:**

**Input:** nums = [1,0,2]

**Output:** true

**Explanation:** There is 1 global inversion and 1 local inversion.

**Example 2:**

**Input:** nums = [1,2,0]

**Output:** false

**Explanation:** There are 2 global inversions and 1 local inversion.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= nums[i] < n`
*   All the integers of `nums` are **unique**.
*   `nums` is a permutation of all the numbers in the range `[0, n - 1]`.

## Solution

```kotlin
class Solution {
    /*
     * from the above solution, we can tell that if we can find the minimum of A[j] where j >= i + 2,
     * then we could quickly return false, so two steps:
     * 1. remembering minimum
     * 2. scanning from right to left
     * <p>
     * Time: O(n)
     */
    fun isIdealPermutation(nums: IntArray): Boolean {
        var min = nums.size
        for (i in nums.size - 1 downTo 2) {
            min = min.coerceAtMost(nums[i])
            if (nums[i - 2] > min) {
                return false
            }
        }
        return true
    }
}
```