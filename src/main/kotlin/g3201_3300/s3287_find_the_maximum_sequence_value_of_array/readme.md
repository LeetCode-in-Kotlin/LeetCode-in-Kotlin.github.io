[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3287\. Find the Maximum Sequence Value of Array

Hard

You are given an integer array `nums` and a **positive** integer `k`.

The **value** of a sequence `seq` of size `2 * x` is defined as:

*   `(seq[0] OR seq[1] OR ... OR seq[x - 1]) XOR (seq[x] OR seq[x + 1] OR ... OR seq[2 * x - 1])`.

Return the **maximum** **value** of any subsequence of `nums` having size `2 * k`.

**Example 1:**

**Input:** nums = [2,6,7], k = 1

**Output:** 5

**Explanation:**

The subsequence `[2, 7]` has the maximum value of `2 XOR 7 = 5`.

**Example 2:**

**Input:** nums = [4,2,5,6,7], k = 2

**Output:** 2

**Explanation:**

The subsequence `[4, 5, 6, 7]` has the maximum value of `(4 OR 5) XOR (6 OR 7) = 2`.

**Constraints:**

*   `2 <= nums.length <= 400`
*   <code>1 <= nums[i] < 2<sup>7</sup></code>
*   `1 <= k <= nums.length / 2`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxValue(nums: IntArray, k: Int): Int {
        val n = nums.size
        val left: Array<Array<MutableSet<Int>>> =
            Array<Array<MutableSet<Int>>>(n) { Array<MutableSet<Int>>(k + 1) { mutableSetOf() } }
        val right: Array<Array<MutableSet<Int>>> =
            Array<Array<MutableSet<Int>>>(n) { Array<MutableSet<Int>>(k + 1) { mutableSetOf() } }
        left[0][0].add(0)
        left[0][1].add(nums[0])
        for (i in 1 until n - k) {
            left[i][0].add(0)
            for (j in 1..k) {
                left[i][j].addAll(left[i - 1][j])
                for (v in left[i - 1][j - 1]) {
                    left[i][j].add(v or nums[i])
                }
            }
        }
        right[n - 1][0].add(0)
        right[n - 1][1].add(nums[n - 1])
        var result = 0
        if (k == 1) {
            for (l in left[n - 2][k]) {
                result = max(result, (l xor nums[n - 1]))
            }
        }
        for (i in n - 2 downTo k) {
            right[i][0].add(0)
            for (j in 1..k) {
                right[i][j].addAll(right[i + 1][j])
                for (v in right[i + 1][j - 1]) {
                    right[i][j].add(v or nums[i])
                }
            }
            for (l in left[i - 1][k]) {
                for (r in right[i][k]) {
                    result = max(result, (l xor r))
                }
            }
        }
        return result
    }
}
```