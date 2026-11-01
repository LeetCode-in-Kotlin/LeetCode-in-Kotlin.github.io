[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3859\. Count Subarrays With K Distinct Integers

Hard

You are given an integer array `nums` and two integers `k` and `m`.

Return an integer denoting the count of **non-empty subarrays** of `nums` such that:

*   The subarray contains **exactly** `k` **distinct** integers.
*   Within the subarray, each **distinct** integer appears **at least** `m` times.

**Example 1:**

**Input:** nums = [1,2,1,2,2], k = 2, m = 2

**Output:** 2

**Explanation:**

The possible subarrays with `k = 2` distinct integers, each appearing at least `m = 2` times are:

| Subarray | Distinct<br>numbers | Frequency |
|---|---|---|
| `[1, 2, 1, 2]` | `{1, 2}` → `2` | `{1: 2, 2: 2}` |
| `[1, 2, 1, 2, 2]` | `{1, 2}` → `2` | `{1: 2, 2: 3}` |

Thus, the answer is 2.

**Example 2:**

**Input:** nums = [3,1,2,4], k = 2, m = 1

**Output:** 3

**Explanation:**

The possible subarrays with `k = 2` distinct integers, each appearing at least `m = 1` times are:

| Subarray | Distinct<br>numbers | Frequency |
|---|---|---|
| `[3, 1]` | `{3, 1}` → `2` | `{3: 1, 1: 1}` |
| `[1, 2]` | `{1, 2}` → `2` | `{1: 1, 2: 1}` |
| `[2, 4]` | `{2, 4}` → `2` | `{2: 1, 4: 1}` |

Thus, the answer is 3.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k, m <= nums.length`

## Solution

```kotlin
class Solution {
    fun countSubarrays(nums: IntArray, k: Int, m: Int): Long {
        var left = 0
        var p = 0
        var subArr: Long = 0
        val map = HashMap<Int?, Int?>()
        var valid = 0
        for (`val` in nums) {
            map[`val`] = map.getOrDefault(`val`, 0)!! + 1
            if (map[`val`] == m) {
                valid++
            }
            val state = shrinkDistinct(nums, left, p, k, m, map, valid)
            left = state.left()
            p = state.p()
            valid = state.valid()
            val duplicateState = trimDuplicates(nums, left, p, m, map)
            left = duplicateState.left()
            p = duplicateState.p()
            if (map.size == k && valid == k) {
                subArr += (1 + p).toLong()
            }
        }
        return subArr
    }

    private fun shrinkDistinct(
        nums: IntArray,
        left: Int,
        p: Int,
        k: Int,
        m: Int,
        map: MutableMap<Int?, Int?>,
        valid: Int,
    ): WindowState {
        var left = left
        var p = p
        var valid = valid
        while (map.size > k) {
            val lv = nums[left]
            if (map[lv] == m) {
                valid--
            }
            map[lv] = map[lv]!! - 1
            if (map[lv] == 0) {
                map.remove(lv)
            }
            left++
            p = 0
        }
        return WindowState(left, p, valid)
    }

    private fun trimDuplicates(nums: IntArray, left: Int, p: Int, m: Int, map: MutableMap<Int?, Int?>): WindowState {
        var left = left
        var p = p
        while (map.isNotEmpty() && map[nums[left]]!! > m) {
            val lv = nums[left]
            map[lv] = map[lv]!! - 1
            left++
            p++
        }
        return WindowState(left, p, 0)
    }

    private class WindowState(private val left: Int, private val p: Int, private val valid: Int) {
        fun left(): Int = left

        fun p(): Int = p

        fun valid(): Int = valid
    }
}
```