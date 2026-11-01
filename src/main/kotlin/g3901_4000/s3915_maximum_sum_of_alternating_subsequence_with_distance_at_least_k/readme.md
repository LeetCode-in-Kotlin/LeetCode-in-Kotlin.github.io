[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3915\. Maximum Sum of Alternating Subsequence With Distance at Least K

Hard

You are given an integer array `nums` of length `n` and an integer `k`.

Pick a **subsequence** with indices <code>0 <= i<sub>1</sub> < i<sub>2</sub> < ... < i<sub>m</sub> < n</code> such that:

*   For every `1 <= t < m`, <code>i<sub>t+1</sub> - i<sub>t</sub> >= k</code>.
*   The selected values form a **strictly alternating** sequence. In other words, either:
    *   <code>nums[i<sub>1</sub>] < nums[i<sub>2</sub>] > nums[i<sub>3</sub>] < ...</code>, or
    *   <code>nums[i<sub>1</sub>] > nums[i<sub>2</sub>] < nums[i<sub>3</sub>] > ...</code>

A **subsequence** of length 1 is also considered **strictly** alternating. The score of a **valid** subsequence is the **sum** of its selected values.

Return an integer denoting the **maximum** possible **score** of a valid subsequence.

**Example 1:**

**Input:** nums = [5,4,2], k = 2

**Output:** 7

**Explanation:**

An optimal choice is indices `[0, 2]`, which gives values `[5, 2]`.

*   The distance condition holds because `2 - 0 = 2 >= k`.
*   The values are strictly alternating because `5 > 2`.

The score is `5 + 2 = 7`.

**Example 2:**

**Input:** nums = [3,5,4,2,4], k = 1

**Output:** 14

**Explanation:**

An optimal choice is indices `[0, 1, 3, 4]`, which gives values `[3, 5, 2, 4]`.

*   The distance condition holds because each pair of consecutive chosen indices differs by at least `k = 1`.
*   The values are strictly alternating since `3 < 5 > 2 < 4`.

The score is `3 + 5 + 2 + 4 = 14`.

**Example 3:**

**Input:** nums = [5], k = 1

**Output:** 5

**Explanation:**

The only valid subsequence is `[5]`. A subsequence with 1 element is always strictly alternating, so the score is 5.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= n`

## Solution

```kotlin
class Solution {
    fun maxAlternatingSum(nums: IntArray, k: Int): Long {
        val n = nums.size
        var maxVal = 0
        // Find the maximum value in nums to define the size of our data structures
        for (x in nums) {
            if (x > maxVal) {
                maxVal = x
            }
        }
        // Variable created as requested to store an input midway in the function
        // peak[i] stores the max alternating sum ending at index i where nums[i] is a peak
        val peak = LongArray(n)
        // valley[i] stores the max alternating sum ending at index i where nums[i] is a valley
        val valley = LongArray(n)
        // Fenwick Trees for Range Maximum Queries
        // prefixBitTree will store valley[j] values to find max valley[j] where nums[j] < nums[i]
        val prefixBitTree = LongArray(maxVal + 1)
        // suffixBitTree will store peak[j] values to find max peak[j] where nums[j] > nums[i]
        // We use the transformation maxVal - nums[j] + 1 to treat suffix max as prefix max
        val suffixBitTree = LongArray(maxVal + 1)
        var maxScore: Long = 0
        for (i in 0..<n) {
            // Distance condition: consecutive indices must differ by at least k
            if (i >= k) {
                // Activate the valid index (i - k) by updating both Fenwick trees
                val valIdxPrefix = nums[i - k]
                val valPrefix = valley[i - k]
                run {
                    var idx = valIdxPrefix
                    while (idx <= maxVal) {
                        if (valPrefix > prefixBitTree[idx]) {
                            prefixBitTree[idx] = valPrefix
                        }
                        idx += idx and -idx
                    }
                }
                val valIdxSuffix = maxVal - nums[i - k] + 1
                val valSuffix = peak[i - k]
                var idx = valIdxSuffix
                while (idx <= maxVal) {
                    if (valSuffix > suffixBitTree[idx]) {
                        suffixBitTree[idx] = valSuffix
                    }
                    idx += idx and -idx
                }
            }
            // Find max alternating sum if nums[i] is the current peak (needs previous valley <
            // nums[i])
            var maxPrevValley: Long = 0
            run {
                var q = nums[i] - 1
                while (q > 0) {
                    if (prefixBitTree[q] > maxPrevValley) {
                        maxPrevValley = prefixBitTree[q]
                    }
                    q -= q and -q
                }
            }
            // A length-1 subsequence is also strictly alternating
            peak[i] = nums[i] + maxPrevValley
            // Find max alternating sum if nums[i] is the current valley (needs previous peak >
            // nums[i])
            var maxPrevPeak: Long = 0
            var q = maxVal - nums[i]
            while (q > 0) {
                if (suffixBitTree[q] > maxPrevPeak) {
                    maxPrevPeak = suffixBitTree[q]
                }
                q -= q and -q
            }
            // A length-1 subsequence is also strictly alternating
            valley[i] = nums[i] + maxPrevPeak
            // Update global maximum score
            if (peak[i] > maxScore) {
                maxScore = peak[i]
            }
            if (valley[i] > maxScore) {
                maxScore = valley[i]
            }
        }
        return maxScore
    }
}
```