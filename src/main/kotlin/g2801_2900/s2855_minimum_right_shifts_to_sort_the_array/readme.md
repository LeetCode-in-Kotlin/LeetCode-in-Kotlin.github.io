[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2855\. Minimum Right Shifts to Sort the Array

Easy

You are given a **0-indexed** array `nums` of length `n` containing **distinct** positive integers. Return _the **minimum** number of **right shifts** required to sort_ `nums` _and_ `-1` _if this is not possible._

A **right shift** is defined as shifting the element at index `i` to index `(i + 1) % n`, for all indices.

**Example 1:**

**Input:** nums = [3,4,5,1,2]

**Output:** 2

**Explanation:** 

After the first right shift, nums = [2,3,4,5,1]. 

After the second right shift, nums = [1,2,3,4,5]. 

Now nums is sorted; therefore the answer is 2.

**Example 2:**

**Input:** nums = [1,3,5]

**Output:** 0

**Explanation:** nums is already sorted therefore, the answer is 0.

**Example 3:**

**Input:** nums = [2,1,4]

**Output:** -1

**Explanation:** It's impossible to sort the array using right shifts.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`
*   `nums` contains distinct integers.

## Solution

```kotlin
@Suppress("kotlin:S6510")
class Solution {
    fun minimumRightShifts(nums: List<Int>): Int {
        var i = 1
        while (i < nums.size) {
            if (nums[i] < nums[i - 1]) {
                break
            }
            i++
        }
        if (nums.size == i) {
            return 0
        } else {
            var k = i + 1
            while (k < nums.size) {
                if (nums[k] <= nums[k - 1]) {
                    break
                }
                k++
            }
            if (k == nums.size && nums[k - 1] < nums[0]) {
                return nums.size - i
            }
            return -1
        }
    }
}
```