[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 697\. Degree of an Array

Easy

Given a non-empty array of non-negative integers `nums`, the **degree** of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of `nums`, that has the same degree as `nums`.

**Example 1:**

**Input:** nums = [1,2,2,3,1]

**Output:** 2

**Explanation:** The input array has a degree of 2 because both elements 1 and 2 appear twice. Of the subarrays that have the same degree: [1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2] The shortest length is 2. So return 2.

**Example 2:**

**Input:** nums = [1,2,2,3,1,4,2]

**Output:** 6

**Explanation:** The degree is 3 because the element 2 is repeated 3 times. So [2,2,3,1,4,2] is the shortest subarray, therefore returning 6.

**Constraints:**

*   `nums.length` will be between 1 and 50,000.
*   `nums[i]` will be an integer between 0 and 49,999.

## Solution

```kotlin
class Solution {
    private class Value(var count: Int, var start: Int, var end: Int)

    fun findShortestSubArray(nums: IntArray): Int {
        var max = 1
        val map: MutableMap<Int, Value> = HashMap()
        for (i in nums.indices) {
            val j = nums[i]
            if (map.containsKey(j)) {
                val v = map[j]
                v!!.count++
                max = Math.max(max, v.count)
                v.end = i
            } else {
                map[j] = Value(1, i, i)
            }
        }
        var min = Int.MAX_VALUE
        for (entry in map.entries.iterator()) {
            val v: Value = entry.value
            if (v.count == max) {
                min = min.coerceAtMost(v.end - v.start)
            }
        }
        return min + 1
    }
}
```