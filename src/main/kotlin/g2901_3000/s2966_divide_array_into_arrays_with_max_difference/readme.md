[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2966\. Divide Array Into Arrays With Max Difference

Medium

You are given an integer array `nums` of size `n` and a positive integer `k`.

Divide the array into one or more arrays of size `3` satisfying the following conditions:

*   **Each** element of `nums` should be in **exactly** one array.
*   The difference between **any** two elements in one array is less than or equal to `k`.

Return _a_ **2D** _array containing all the arrays. If it is impossible to satisfy the conditions, return an empty array. And if there are multiple answers, return **any** of them._

**Example 1:**

**Input:** nums = [1,3,4,8,7,9,3,5,1], k = 2

**Output:** [[1,1,3],[3,4,5],[7,8,9]]

**Explanation:** We can divide the array into the following arrays: [1,1,3], [3,4,5] and [7,8,9]. The difference between any two elements in each array is less than or equal to 2. Note that the order of elements is not important.

**Example 2:**

**Input:** nums = [1,3,3,2,7,3], k = 3

**Output:** []

**Explanation:** It is not possible to divide the array satisfying all the conditions.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `n` is a multiple of `3`.
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun divideArray(nums: IntArray, k: Int): Array<IntArray> {
        nums.sort()
        val n = nums.size
        val triplets = n / 3
        val result = Array(triplets) { intArrayOf() }
        var i = 0
        var j = 0
        while (i < n) {
            val first = nums[i]
            val third = nums[i + 2]
            if (third - first > k) {
                return Array(0) { intArrayOf() }
            }
            result[j] = intArrayOf(first, nums[i + 1], third)
            i += 3
            j++
        }
        return result
    }
}
```