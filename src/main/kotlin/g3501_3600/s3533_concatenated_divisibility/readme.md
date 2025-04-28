[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3533\. Concatenated Divisibility

Hard

You are given an array of positive integers `nums` and a positive integer `k`.

A permutation of `nums` is said to form a **divisible concatenation** if, when you _concatenate_ _the decimal representations_ of the numbers in the order specified by the permutation, the resulting number is **divisible by** `k`.

Return the **lexicographically smallest** permutation (when considered as a list of integers) that forms a **divisible concatenation**. If no such permutation exists, return an empty list.

**Example 1:**

**Input:** nums = [3,12,45], k = 5

**Output:** [3,12,45]

**Explanation:**

| Permutation | Concatenated Value | Divisible by 5 |
|-------------|--------------------|----------------|
| [3, 12, 45] | 31245              | Yes            |
| [3, 45, 12] | 34512              | No             |
| [12, 3, 45] | 12345              | Yes            |
| [12, 45, 3] | 12453              | No             |
| [45, 3, 12] | 45312              | No             |
| [45, 12, 3] | 45123              | No             |

The lexicographically smallest permutation that forms a divisible concatenation is `[3,12,45]`.

**Example 2:**

**Input:** nums = [10,5], k = 10

**Output:** [5,10]

**Explanation:**

| Permutation | Concatenated Value | Divisible by 10 |
|-------------|--------------------|-----------------|
| [5, 10]     | 510                | Yes             |
| [10, 5]     | 105                | No              |

The lexicographically smallest permutation that forms a divisible concatenation is `[5,10]`.

**Example 3:**

**Input:** nums = [1,2,3], k = 5

**Output:** []

**Explanation:**

Since no permutation of `nums` forms a valid divisible concatenation, return an empty list.

**Constraints:**

*   `1 <= nums.length <= 13`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= 100`

## Solution

```kotlin
@Suppress("kotlin:S107")
class Solution {
    fun concatenatedDivisibility(nums: IntArray, k: Int): IntArray {
        nums.sort()
        var digits = 0
        val n = nums.size
        val digCnt = IntArray(n)
        for (i in 0..<n) {
            var num = nums[i]
            digits++
            digCnt[i]++
            while (num >= 10) {
                digits++
                digCnt[i]++
                num /= 10
            }
        }
        val pow10 = IntArray(digits + 1)
        pow10[0] = 1
        for (i in 1..digits) {
            pow10[i] = (pow10[i - 1] * 10) % k
        }
        val res = IntArray(n)
        return if (dfs(0, 0, k, digCnt, nums, pow10, Array(1 shl n) { BooleanArray(k) }, 0, res, n)) {
            res
        } else {
            IntArray(0)
        }
    }

    private fun dfs(
        mask: Int,
        residue: Int,
        k: Int,
        digCnt: IntArray,
        nums: IntArray,
        pow10: IntArray,
        visited: Array<BooleanArray>,
        ansIdx: Int,
        ans: IntArray,
        n: Int,
    ): Boolean {
        if (ansIdx == n) {
            return residue == 0
        }
        if (visited[mask][residue]) {
            return false
        }
        var i = 0
        var bit = 1
        while (i < n) {
            if ((mask and bit) == bit) {
                i++
                bit = bit shl 1
                continue
            }
            val newResidue = (residue * pow10[digCnt[i]] + nums[i]) % k
            ans[ansIdx] = nums[i]
            if (dfs(mask or bit, newResidue, k, digCnt, nums, pow10, visited, ansIdx + 1, ans, n)) {
                return true
            }
            i++
            bit = bit shl 1
        }
        visited[mask][residue] = true
        return false
    }
}
```