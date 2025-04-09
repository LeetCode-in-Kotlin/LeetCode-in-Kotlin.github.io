[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3507\. Minimum Pair Removal to Sort Array I

Easy

Given an array `nums`, you can perform the following operation any number of times:

*   Select the **adjacent** pair with the **minimum** sum in `nums`. If multiple such pairs exist, choose the leftmost one.
*   Replace the pair with their sum.

Return the **minimum number of operations** needed to make the array **non-decreasing**.

An array is said to be **non-decreasing** if each element is greater than or equal to its previous element (if it exists).

**Example 1:**

**Input:** nums = [5,2,3,1]

**Output:** 2

**Explanation:**

*   The pair `(3,1)` has the minimum sum of 4. After replacement, `nums = [5,2,4]`.
*   The pair `(2,4)` has the minimum sum of 6. After replacement, `nums = [5,6]`.

The array `nums` became non-decreasing in two operations.

**Example 2:**

**Input:** nums = [1,2,2]

**Output:** 0

**Explanation:**

The array `nums` is already sorted.

**Constraints:**

*   `1 <= nums.length <= 50`
*   `-1000 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun minimumPairRemoval(nums: IntArray): Int {
        var nums = nums
        var operations = 0
        while (!isNonDecreasing(nums)) {
            var minSum = Int.Companion.MAX_VALUE
            var index = 0
            // Find the leftmost pair with minimum sum
            for (i in 0..<nums.size - 1) {
                val sum = nums[i] + nums[i + 1]
                if (sum < minSum) {
                    minSum = sum
                    index = i
                }
            }
            // Merge the pair at index
            val newNums = IntArray(nums.size - 1)
            var j = 0
            var i = 0
            while (i < nums.size) {
                if (i == index) {
                    newNums[j++] = nums[i] + nums[i + 1]
                    // Skip the next one since it's merged
                    i++
                } else {
                    newNums[j++] = nums[i]
                }
                i++
            }
            nums = newNums
            operations++
        }
        return operations
    }

    private fun isNonDecreasing(nums: IntArray): Boolean {
        for (i in 1..<nums.size) {
            if (nums[i] < nums[i - 1]) {
                return false
            }
        }
        return true
    }
}
```