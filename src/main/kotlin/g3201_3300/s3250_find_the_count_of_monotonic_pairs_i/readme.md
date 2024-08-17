[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3250\. Find the Count of Monotonic Pairs I

Hard

You are given an array of **positive** integers `nums` of length `n`.

We call a pair of **non-negative** integer arrays `(arr1, arr2)` **monotonic** if:

*   The lengths of both arrays are `n`.
*   `arr1` is monotonically **non-decreasing**, in other words, `arr1[0] <= arr1[1] <= ... <= arr1[n - 1]`.
*   `arr2` is monotonically **non-increasing**, in other words, `arr2[0] >= arr2[1] >= ... >= arr2[n - 1]`.
*   `arr1[i] + arr2[i] == nums[i]` for all `0 <= i <= n - 1`.

Return the count of **monotonic** pairs.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [2,3,2]

**Output:** 4

**Explanation:**

The good pairs are:

1.  `([0, 1, 1], [2, 2, 1])`
2.  `([0, 1, 2], [2, 2, 0])`
3.  `([0, 2, 2], [2, 1, 0])`
4.  `([1, 2, 2], [1, 1, 0])`

**Example 2:**

**Input:** nums = [5,5,5,5]

**Output:** 126

**Constraints:**

*   `1 <= n == nums.length <= 2000`
*   `1 <= nums[i] <= 50`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun countOfPairs(nums: IntArray): Int {
        val maxShift = IntArray(nums.size)
        maxShift[0] = nums[0]
        var currShift = 0
        for (i in 1 until nums.size) {
            currShift = max(currShift, (nums[i] - maxShift[i - 1]))
            maxShift[i] = min(maxShift[i - 1], (nums[i] - currShift))
            if (maxShift[i] < 0) {
                return 0
            }
        }
        val cases = getAllCases(nums, maxShift)
        return cases[nums.size - 1]!![maxShift[nums.size - 1]]
    }

    private fun getAllCases(nums: IntArray, maxShift: IntArray): Array<IntArray?> {
        var currCases: IntArray
        val cases = arrayOfNulls<IntArray>(nums.size)
        cases[0] = IntArray(maxShift[0] + 1)
        for (i in cases[0]!!.indices) {
            cases[0]!![i] = i + 1
        }
        for (i in 1 until nums.size) {
            currCases = IntArray(maxShift[i] + 1)
            currCases[0] = 1
            for (j in 1 until currCases.size) {
                val prevCases =
                    if (j < cases[i - 1]!!.size
                    ) cases[i - 1]!![j]
                    else cases[i - 1]!![cases[i - 1]!!.size - 1]
                currCases[j] = (currCases[j - 1] + prevCases) % (1000000000 + 7)
            }
            cases[i] = currCases
        }
        return cases
    }
}
```