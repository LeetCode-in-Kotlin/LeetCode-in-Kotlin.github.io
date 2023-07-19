[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2610\. Convert an Array Into a 2D Array With Conditions

Medium

You are given an integer array `nums`. You need to create a 2D array from `nums` satisfying the following conditions:

*   The 2D array should contain **only** the elements of the array `nums`.
*   Each row in the 2D array contains **distinct** integers.
*   The number of rows in the 2D array should be **minimal**.

Return _the resulting array_. If there are multiple answers, return any of them.

**Note** that the 2D array can have a different number of elements on each row.

**Example 1:**

**Input:** nums = [1,3,4,1,2,3,1]

**Output:** [[1,3,4,2],[1,3],[1]]

**Explanation:** We can create a 2D array that contains the following rows: - 1,3,4,2 - 1,3 - 1 All elements of nums were used, and each row of the 2D array contains distinct integers, so it is a valid answer. It can be shown that we cannot have less than 3 rows in a valid array.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** [[4,3,2,1]]

**Explanation:** All elements of the array are distinct, so we can keep all of them in the first row of the 2D array.

**Constraints:**

*   `1 <= nums.length <= 200`
*   `1 <= nums[i] <= nums.length`

## Solution

```kotlin
class Solution {
    fun findMatrix(nums: IntArray): List<List<Int>> {
        val ans = mutableListOf<MutableList<Int>>()
        val map = mutableMapOf<Int, Int>()
        var max = 0

        for (n in nums) {
            val count = map.getOrDefault(n, 0) + 1
            map[n] = count
            max = max.coerceAtLeast(count)
        }

        repeat(max) {
            val new = mutableListOf<Int>()
            for (e in map) {
                if (e.value != 0) {
                    new.add(e.key)
                    map[e.key] = e.value - 1
                }
            }
            ans.add(new)
        }

        return ans
    }
}
```