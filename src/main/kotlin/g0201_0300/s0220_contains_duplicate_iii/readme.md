[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 220\. Contains Duplicate III

Hard

You are given an integer array `nums` and two integers `indexDiff` and `valueDiff`.

Find a pair of indices `(i, j)` such that:

*   `i != j`,
*   `abs(i - j) <= indexDiff`.
*   `abs(nums[i] - nums[j]) <= valueDiff`, and

Return `true` _if such pair exists or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [1,2,3,1], indexDiff = 3, valueDiff = 0

**Output:** true

**Explanation:** We can choose (i, j) = (0, 3). 

We satisfy the three conditions: 

i != j --> 0 != 3 

abs(i - j) <= indexDiff --> 

abs(0 - 3) <= 3 

abs(nums[i] - nums[j]) <= valueDiff --> abs(1 - 1) <= 0

**Example 2:**

**Input:** nums = [1,5,9,1,5,9], indexDiff = 2, valueDiff = 3

**Output:** false

**Explanation:** After trying all the possible pairs (i, j), we cannot satisfy the three conditions, so we return false.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= indexDiff <= nums.length`
*   <code>0 <= valueDiff <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    private fun getId(i: Long, w: Long): Long {
        return if (i < 0) (i + 1) / w - 1 else i / w
    }

    fun containsNearbyAlmostDuplicate(nums: IntArray, k: Int, t: Int): Boolean {
        if (t < 0) {
            return false
        }
        val d: MutableMap<Long, Long> = HashMap()
        val w = t.toLong() + 1
        for (i in nums.indices) {
            val m = getId(nums[i].toLong(), w)
            if (d.containsKey(m)) {
                return true
            }
            if (d.containsKey(m - 1) && Math.abs(nums[i] - d.getValue(m - 1)) < w) {
                return true
            }
            if (d.containsKey(m + 1) && Math.abs(nums[i] - d.getValue(m + 1)) < w) {
                return true
            }
            d[m] = nums[i].toLong()
            if (i >= k) {
                d.remove(getId(nums[i - k].toLong(), w))
            }
        }
        return false
    }
}
```