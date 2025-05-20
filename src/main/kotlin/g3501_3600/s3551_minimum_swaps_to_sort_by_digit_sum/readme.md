[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3551\. Minimum Swaps to Sort by Digit Sum

Medium

You are given an array `nums` of **distinct** positive integers. You need to sort the array in **increasing** order based on the sum of the digits of each number. If two numbers have the same digit sum, the **smaller** number appears first in the sorted order.

Return the **minimum** number of swaps required to rearrange `nums` into this sorted order.

A **swap** is defined as exchanging the values at two distinct positions in the array.

**Example 1:**

**Input:** nums = [37,100]

**Output:** 1

**Explanation:**

*   Compute the digit sum for each integer: `[3 + 7 = 10, 1 + 0 + 0 = 1] → [10, 1]`
*   Sort the integers based on digit sum: `[100, 37]`. Swap `37` with `100` to obtain the sorted order.
*   Thus, the minimum number of swaps required to rearrange `nums` is 1.

**Example 2:**

**Input:** nums = [22,14,33,7]

**Output:** 0

**Explanation:**

*   Compute the digit sum for each integer: `[2 + 2 = 4, 1 + 4 = 5, 3 + 3 = 6, 7 = 7] → [4, 5, 6, 7]`
*   Sort the integers based on digit sum: `[22, 14, 33, 7]`. The array is already sorted.
*   Thus, the minimum number of swaps required to rearrange `nums` is 0.

**Example 3:**

**Input:** nums = [18,43,34,16]

**Output:** 2

**Explanation:**

*   Compute the digit sum for each integer: `[1 + 8 = 9, 4 + 3 = 7, 3 + 4 = 7, 1 + 6 = 7] → [9, 7, 7, 7]`
*   Sort the integers based on digit sum: `[16, 34, 43, 18]`. Swap `18` with `16`, and swap `43` with `34` to obtain the sorted order.
*   Thus, the minimum number of swaps required to rearrange `nums` is 2.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `nums` consists of **distinct** positive integers.

## Solution

```kotlin
class Solution {
    private class Pair(var sum: Int, var value: Int, var index: Int)

    fun minSwaps(arr: IntArray): Int {
        val n = arr.size
        val pairs = arrayOfNulls<Pair>(n)
        for (i in 0..<n) {
            var v = arr[i]
            var s = 0
            while (v > 0) {
                s += v % 10
                v /= 10
            }
            pairs[i] = Pair(s, arr[i], i)
        }
        pairs.sortWith { a, b ->
            if (a!!.sum != b!!.sum) {
                a.sum - b.sum
            } else {
                a.value - b.value
            }
        }
        val posMap = IntArray(n)
        for (i in 0..<n) {
            posMap[i] = pairs[i]!!.index
        }
        val seen = BooleanArray(n)
        var swaps = 0
        for (i in 0..<n) {
            if (seen[i] || posMap[i] == i) {
                continue
            }
            var cycleSize = 0
            var j = i
            while (!seen[j]) {
                seen[j] = true
                j = posMap[j]
                cycleSize++
            }
            swaps += cycleSize - 1
        }
        return swaps
    }
}
```