[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 446\. Arithmetic Slices II - Subsequence

Hard

Given an integer array `nums`, return _the number of all the **arithmetic subsequences** of_ `nums`.

A sequence of numbers is called arithmetic if it consists of **at least three elements** and if the difference between any two consecutive elements is the same.

*   For example, `[1, 3, 5, 7, 9]`, `[7, 7, 7, 7]`, and `[3, -1, -5, -9]` are arithmetic sequences.
*   For example, `[1, 1, 2, 5, 7]` is not an arithmetic sequence.

A **subsequence** of an array is a sequence that can be formed by removing some elements (possibly none) of the array.

*   For example, `[2,5,10]` is a subsequence of `[1,2,1,**2**,4,1,**5**,**10**]`.

The test cases are generated so that the answer fits in **32-bit** integer.

**Example 1:**

**Input:** nums = [2,4,6,8,10]

**Output:** 7

**Explanation:** All arithmetic subsequence slices are:

[2,4,6]

[4,6,8]

[6,8,10]

[2,4,6,8]

[4,6,8,10]

[2,4,6,8,10]

[2,6,10]

**Example 2:**

**Input:** nums = [7,7,7,7,7]

**Output:** 16

**Explanation:** Any subsequence of this array is arithmetic.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun numberOfArithmeticSlices(arr: IntArray): Int {
        val indexes: MutableMap<Long, MutableList<Int>> = HashMap()
        val length = Array(arr.size) { IntArray(arr.size) }
        var count = 0
        for (i in arr.indices) {
            for (j in i + 1 until arr.size) {
                val ix = indexes[arr[i] - (arr[j] - arr[i].toLong())] ?: continue
                for (k in ix) {
                    length[i][j] += length[k][i] + 1
                }
                count += length[i][j]
            }
            indexes.computeIfAbsent(
                arr[i].toLong(),
            ) { _: Long? -> ArrayList() }.add(i)
        }
        return count
    }
}
```