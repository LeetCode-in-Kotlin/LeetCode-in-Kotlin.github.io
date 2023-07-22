[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2659\. Make Array Empty

Hard

You are given an integer array `nums` containing **distinct** numbers, and you can perform the following operations **until the array is empty**:

*   If the first element has the **smallest** value, remove it
*   Otherwise, put the first element at the **end** of the array.

Return _an integer denoting the number of operations it takes to make_ `nums` _empty._

**Example 1:**

**Input:** nums = [3,4,-1]

**Output:** 5

    Operation   Array
    1           [4, -1, 3]
    2           [-1, 3, 4]
    3           [3, 4]
    4           [4]
    5           []

**Example 2:**

**Input:** nums = [1,2,4,3]

**Output:** 5

    Operation   Array
    1           [2, 4, 3]
    2           [4, 3]
    3           [3, 4]
    4           [4]
    5           []

**Example 3:**

**Input:** nums = [1,2,3]

**Output:** 3

    Operation   Array
    1           [2, 3]
    2           [3]
    3           []

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9 </sup><= nums[i] <= 10<sup>9</sup></code>
*   All values in `nums` are **distinct**.

## Solution

```kotlin
class Solution {
    fun countOperationsToEmptyArray(nums: IntArray): Long {
        val sortNums = Array(nums.size) { IntArray(2) }
        for (i in nums.indices) {
            sortNums[i][0] = nums[i]
            sortNums[i][1] = i
        }
        sortNums.sortBy { it[0] }
        var res = 0L + nums.size
        for (i in 1..sortNums.lastIndex) {
            if (sortNums[i - 1][1] > sortNums[i][1])
                res += nums.size - i
        }
        return res
    }
}
```