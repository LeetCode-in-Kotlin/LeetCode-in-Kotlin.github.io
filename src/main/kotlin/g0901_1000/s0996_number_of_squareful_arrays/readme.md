[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 996\. Number of Squareful Arrays

Hard

An array is **squareful** if the sum of every pair of adjacent elements is a **perfect square**.

Given an integer array nums, return _the number of permutations of_ `nums` _that are **squareful**_.

Two permutations `perm1` and `perm2` are different if there is some index `i` such that `perm1[i] != perm2[i]`.

**Example 1:**

**Input:** nums = [1,17,8]

**Output:** 2

**Explanation:** [1,8,17] and [17,8,1] are the valid permutations.

**Example 2:**

**Input:** nums = [2,2,2]

**Output:** 1

**Constraints:**

*   `1 <= nums.length <= 12`
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.sqrt

class Solution {
    var count = 0
    fun numSquarefulPerms(nums: IntArray): Int {
        val n = nums.size
        if (n < 2) {
            return count
        }
        backtrack(nums, n, 0)
        return count
    }

    private fun backtrack(nums: IntArray, n: Int, start: Int) {
        if (start == n) {
            count++
        }
        val set: MutableSet<Int> = HashSet()
        for (i in start until n) {
            if (set.contains(nums[i])) {
                continue
            }
            swap(nums, start, i)
            if (start == 0 || isPerfectSq(nums[start], nums[start - 1])) {
                backtrack(nums, n, start + 1)
            }
            swap(nums, start, i)
            set.add(nums[i])
        }
    }

    private fun swap(array: IntArray, a: Int, b: Int) {
        val temp = array[a]
        array[a] = array[b]
        array[b] = temp
    }

    private fun isPerfectSq(a: Int, b: Int): Boolean {
        val x = a + b
        val sqrt = sqrt(x.toDouble())
        return sqrt - sqrt.toInt() == 0.0
    }
}
```