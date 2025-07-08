[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3605\. Minimum Stability Factor of Array

Hard

You are given an integer array `nums` and an integer `maxC`.

A **subarray** is called **stable** if the _highest common factor (HCF)_ of all its elements is **greater than or equal to** 2.

The **stability factor** of an array is defined as the length of its **longest** stable subarray.

You may modify **at most** `maxC` elements of the array to any integer.

Return the **minimum** possible stability factor of the array after at most `maxC` modifications. If no stable subarray remains, return 0.

**Note:**

*   The **highest common factor (HCF)** of an array is the largest integer that evenly divides all the array elements.
*   A **subarray** of length 1 is stable if its only element is greater than or equal to 2, since `HCF([x]) = x`.

**Example 1:**

**Input:** nums = [3,5,10], maxC = 1

**Output:** 1

**Explanation:**

*   The stable subarray `[5, 10]` has `HCF = 5`, which has a stability factor of 2.
*   Since `maxC = 1`, one optimal strategy is to change `nums[1]` to `7`, resulting in `nums = [3, 7, 10]`.
*   Now, no subarray of length greater than 1 has `HCF >= 2`. Thus, the minimum possible stability factor is 1.

**Example 2:**

**Input:** nums = [2,6,8], maxC = 2

**Output:** 1

**Explanation:**

*   The subarray `[2, 6, 8]` has `HCF = 2`, which has a stability factor of 3.
*   Since `maxC = 2`, one optimal strategy is to change `nums[1]` to 3 and `nums[2]` to 5, resulting in `nums = [2, 3, 5]`.
*   Now, no subarray of length greater than 1 has `HCF >= 2`. Thus, the minimum possible stability factor is 1.

**Example 3:**

**Input:** nums = [2,4,9,6], maxC = 1

**Output:** 2

**Explanation:**

*   The stable subarrays are:
    *   `[2, 4]` with `HCF = 2` and stability factor of 2.
    *   `[9, 6]` with `HCF = 3` and stability factor of 2.
*   Since `maxC = 1`, the stability factor of 2 cannot be reduced due to two separate stable subarrays. Thus, the minimum possible stability factor is 2.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `0 <= maxC <= n`

## Solution

```kotlin
class Solution {
    fun minStable(nums: IntArray, maxC: Int): Int {
        val n = nums.size
        var cnt = 0
        var idx = 0
        while (idx < n) {
            cnt += if (nums[idx] >= 2) 1 else 0
            idx++
        }
        if (cnt <= maxC) {
            return 0
        }
        val logs = IntArray(n + 1)
        var maxLog = 0
        var temp = n
        while (temp > 0) {
            maxLog++
            temp = temp shr 1
        }
        val table = Array<IntArray>(maxLog + 1) { IntArray(n) }
        buildLogs(logs, n)
        buildTable(table, nums, n, maxLog)
        return binarySearch(nums, maxC, n, logs, table)
    }

    private fun buildLogs(logs: IntArray, n: Int) {
        var i = 2
        while (i <= n) {
            logs[i] = logs[i shr 1] + 1
            i++
        }
    }

    private fun buildTable(table: Array<IntArray>, nums: IntArray, n: Int, maxLog: Int) {
        System.arraycopy(nums, 0, table[0], 0, n)
        var level = 1
        while (level <= maxLog) {
            var start = 0
            while (start + (1 shl level) <= n) {
                table[level][start] =
                    gcd(table[level - 1][start], table[level - 1][start + (1 shl (level - 1))])
                start++
            }
            level++
        }
    }

    private fun binarySearch(nums: IntArray, maxC: Int, n: Int, logs: IntArray, table: Array<IntArray>): Int {
        var left = 1
        var right = n
        var result = n
        while (left <= right) {
            val mid = left + ((right - left) shr 1)
            val valid = isValid(nums, maxC, mid, logs, table)
            result = if (valid) mid else result
            val newLeft = if (valid) left else mid + 1
            val newRight = if (valid) mid - 1 else right
            left = newLeft
            right = newRight
        }
        return result
    }

    private fun isValid(arr: IntArray, limit: Int, segLen: Int, logs: IntArray, table: Array<IntArray>): Boolean {
        val n = arr.size
        val window = segLen + 1
        var cuts = 0
        var prevCut = -1
        var pos = 0
        while (pos + window - 1 < n && cuts <= limit) {
            val rangeGcd = getRangeGcd(pos, pos + window - 1, logs, table)
            if (rangeGcd >= 2) {
                val shouldCut = prevCut < pos
                cuts += if (shouldCut) 1 else 0
                prevCut = if (shouldCut) pos + window - 1 else prevCut
            }
            pos++
        }
        return cuts <= limit
    }

    private fun getRangeGcd(left: Int, right: Int, logs: IntArray, table: Array<IntArray>): Int {
        val k = logs[right - left + 1]
        return gcd(table[k][left], table[k][right - (1 shl k) + 1])
    }

    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) a else gcd(b, a % b)
    }
}
```