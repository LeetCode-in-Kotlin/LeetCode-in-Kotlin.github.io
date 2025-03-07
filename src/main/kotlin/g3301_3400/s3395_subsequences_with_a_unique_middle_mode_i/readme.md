[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3395\. Subsequences with a Unique Middle Mode I

Hard

Given an integer array `nums`, find the number of subsequences of size 5 of `nums` with a **unique middle mode**.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **mode** of a sequence of numbers is defined as the element that appears the **maximum** number of times in the sequence.

A sequence of numbers contains a **unique mode** if it has only one mode.

A sequence of numbers `seq` of size 5 contains a **unique middle mode** if the _middle element_ (`seq[2]`) is a **unique mode**.

A **subsequence** is a **non-empty** array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [1,1,1,1,1,1]

**Output:** 6

**Explanation:**

`[1, 1, 1, 1, 1]` is the only subsequence of size 5 that can be formed, and it has a unique middle mode of 1. This subsequence can be formed in 6 different ways, so the output is 6.

**Example 2:**

**Input:** nums = [1,2,2,3,3,4]

**Output:** 4

**Explanation:**

`[1, 2, 2, 3, 4]` and `[1, 2, 3, 3, 4]` each have a unique middle mode because the number at index 2 has the greatest frequency in the subsequence. `[1, 2, 2, 3, 3]` does not have a unique middle mode because 2 and 3 appear twice.

**Example 3:**

**Input:** nums = [0,1,2,3,4,5,6,7,8]

**Output:** 0

**Explanation:**

There is no subsequence of length 5 with a unique middle mode.

**Constraints:**

*   `5 <= nums.length <= 1000`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    private val c2 = LongArray(1001)

    fun subsequencesWithMiddleMode(nums: IntArray): Int {
        if (c2[2] == 0L) {
            c2[0] = 0
            c2[1] = 0
            c2[2] = 1
            for (i in 3..<c2.size) {
                c2[i] = (i * (i - 1) / 2).toLong()
            }
        }
        val n = nums.size
        val newNums = IntArray(n)
        val map: MutableMap<Int, Int> = HashMap<Int, Int>(n)
        var m = 0
        var index = 0
        for (x in nums) {
            var id = map[x]
            if (id == null) {
                id = m++
                map.put(x, id)
            }
            newNums[index++] = id
        }
        if (m == n) {
            return 0
        }
        val rightCount = IntArray(m)
        for (x in newNums) {
            rightCount[x]++
        }
        val leftCount = IntArray(m)
        var ans = n.toLong() * (n - 1) * (n - 2) * (n - 3) * (n - 4) / 120
        for (left in 0..<n - 2) {
            val x = newNums[left]
            rightCount[x]--
            if (left >= 2) {
                val right = n - (left + 1)
                val leftX = leftCount[x]
                val rightX = rightCount[x]
                ans -= c2[left - leftX] * c2[right - rightX]
                for (y in 0..<m) {
                    if (y == x) {
                        continue
                    }
                    val rightY = rightCount[y]
                    val leftY = leftCount[y]
                    ans -= c2[leftY] * rightX * (right - rightX)
                    ans -= c2[rightY] * leftX * (left - leftX)
                    ans -=
                        (
                            leftY
                                * rightY
                                * (
                                    leftX * (right - rightX - rightY) +
                                        rightX * (left - leftX - leftY)
                                    )
                            ).toLong()
                }
            }
            leftCount[x]++
        }
        return (ans % MOD).toInt()
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```