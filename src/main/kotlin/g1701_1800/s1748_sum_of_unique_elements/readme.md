[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1748\. Sum of Unique Elements

Easy

You are given an integer array `nums`. The unique elements of an array are the elements that appear **exactly once** in the array.

Return _the **sum** of all the unique elements of_ `nums`.

**Example 1:**

**Input:** nums = [1,2,3,2]

**Output:** 4

**Explanation:** The unique elements are [1,3], and the sum is 4.

**Example 2:**

**Input:** nums = [1,1,1,1,1]

**Output:** 0

**Explanation:** There are no unique elements, and the sum is 0.

**Example 3:**

**Input:** nums = [1,2,3,4,5]

**Output:** 15

**Explanation:** The unique elements are [1,2,3,4,5], and the sum is 15.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun sumOfUnique(nums: IntArray): Int {
        val map: MutableMap<Int, Int> = HashMap()
        var sum: Int = 0
        for (num: Int in nums) {
            map.put(num, map.getOrDefault(num, 0) + 1)
        }
        for (entry: Map.Entry<Int, Int> in map.entries) {
            if (entry.value == 1) {
                sum += entry.key
            }
        }
        return sum
    }
}
```