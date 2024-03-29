[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3020\. Find the Maximum Number of Elements in Subset

Medium

You are given an array of **positive** integers `nums`.

You need to select a subset of `nums` which satisfies the following condition:

*   You can place the selected elements in a **0-indexed** array such that it follows the pattern: <code>[x, x<sup>2</sup>, x<sup>4</sup>, ..., x<sup>k/2</sup>, x<sup>k</sup>, x<sup>k/2</sup>, ..., x<sup>4</sup>, x<sup>2</sup>, x]</code> (**Note** that `k` can be be any **non-negative** power of `2`). For example, `[2, 4, 16, 4, 2]` and `[3, 9, 3]` follow the pattern while `[2, 4, 8, 4, 2]` does not.

Return _the **maximum** number of elements in a subset that satisfies these conditions._

**Example 1:**

**Input:** nums = [5,4,1,2,2]

**Output:** 3

**Explanation:** We can select the subset {4,2,2}, which can be placed in the array as [2,4,2] which follows the pattern and 2<sup>2</sup> == 4. Hence the answer is 3.

**Example 2:**

**Input:** nums = [1,3,2,4]

**Output:** 1

**Explanation:** We can select the subset {1}, which can be placed in the array as [1] which follows the pattern. Hence the answer is 1. Note that we could have also selected the subsets {2}, {3}, or {4}, there may be multiple subsets which provide the same answer.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumLength(nums: IntArray): Int {
        return withHashMap(nums)
    }

    private fun withHashMap(nums: IntArray): Int {
        val map: MutableMap<Int, Int> = HashMap()
        for (i in nums) {
            map[i] = map.getOrDefault(i, 0) + 1
        }
        var ans = 0
        if (map.containsKey(1)) {
            ans = if (map[1]!! % 2 == 0) {
                map[1]!! - 1
            } else {
                map[1]!!
            }
        }
        for (key in map.keys) {
            if (key == 1) {
                continue
            }
            val len = findSeries(map, key)
            ans = max(ans, len)
        }
        return ans
    }

    private fun findSeries(map: Map<Int, Int>, key: Int): Int {
        val sqr = key * key
        return if (map.containsKey(sqr)) {
            if (map[key]!! >= 2) {
                2 + findSeries(map, sqr)
            } else {
                1
            }
        } else {
            1
        }
    }
}
```