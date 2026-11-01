[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3953\. Maximum Score with Co-Prime Element

Hard

You are given an integer array `nums` of length `n` and an integer `maxVal`.

You **may** change any element in `nums` to any positive integer **less than or equal** to `maxVal`. Each such change costs 1.

Two integers are **co-prime** if their **greatest common divisor (GCD)** is 1.

After all modifications, you **must** choose an index `i` such that, `nums[i]` is **co-prime** with every other element `nums[j]`.

Let:

*   `selectedValue` be the final value of `nums[i]` after modifications.
*   `modificationCost` be the total number of elements changed.

The score is defined as `score = selectedValue - modificationCost`.

Return the **maximum** possible score.

**Example 1:**

**Input:** nums = [3,4,6], maxVal = 5

**Output:** 4

**Explanation:**

Change `nums[2]` from 6 to 5, which costs 1. Choose `nums[2] = 5`, since it is co-prime with 3 and 4.

*   `selectedValue = 5`
*   `modificationCost = 1`
*   The score is `5 - 1 = 4`

**Example 2:**

**Input:** nums = [1,2,3], maxVal = 4

**Output:** 3

**Explanation:**

No modifications are required. Choose `nums[2] = 3`, since it is co-prime with 1 and 2.

*   `selectedValue = 3`
*   `modificationCost = 0`
*   The score is `3 - 0 = 3`

**Example 3:**

**Input:** nums = [2,2], maxVal = 1

**Output:** 1

**Explanation:**

Change `nums[0]` from 2 to 1, which costs 1. Choose `nums[1] = 2`, since it is co-prime with 1.

*   `selectedValue = 2`
*   `modificationCost = 1`
*   The score is `2 - 1 = 1`

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= maxVal <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxScore(nums: IntArray, maxVal: Int): Int {
        val max = findMax(nums, maxVal)
        val cnt = countNumbers(nums, max)
        val groupCnt = buildGroupCounts(cnt, max)
        val best = findBestAboveMaxVal(max, maxVal, cnt, groupCnt)
        return findBestWithinMaxVal(maxVal, cnt, groupCnt, best)
    }

    private fun findMax(nums: IntArray, maxVal: Int): Int {
        var max = maxVal
        for (num in nums) {
            if (num > max) {
                max = num
            }
        }
        return max
    }

    private fun countNumbers(nums: IntArray, max: Int): IntArray {
        val cnt = IntArray(max + 1)
        for (num in nums) {
            cnt[num]++
        }
        return cnt
    }

    private fun buildGroupCounts(cnt: IntArray, max: Int): IntArray {
        val groupCnt = IntArray(max + 1)
        for (i in 1..max) {
            var j = 1
            while (j * i <= max) {
                groupCnt[i] += cnt[j * i]
                j++
            }
        }
        return groupCnt
    }

    private fun findBestAboveMaxVal(max: Int, maxVal: Int, cnt: IntArray, groupCnt: IntArray): Int {
        var best = -1
        for (i in max downTo maxVal + 1) {
            if (cnt[i] == 0) {
                continue
            }
            val change = countConflict(i, groupCnt)
            best = max(best, i - change + 1)
        }
        return best
    }

    private fun findBestWithinMaxVal(maxVal: Int, cnt: IntArray, groupCnt: IntArray, best: Int): Int {
        var best = best
        for (i in maxVal downTo 1) {
            if (best != -1 && i <= best) {
                break
            }
            var change = countConflict(i, groupCnt)
            change = adjustChange(i, change, cnt)
            best = max(best, i - change)
        }
        return best
    }

    private fun adjustChange(target: Int, change: Int, cnt: IntArray): Int {
        var change = change
        if (change > 0 && cnt[target] != 0) {
            change--
        }
        if (change == 0 && cnt[target] == 0) {
            change++
        }
        return change
    }

    private fun countConflict(target: Int, groupCnt: IntArray): Int {
        val primeFactors = getFactor(target.toLong())
        var change = 0
        val subsetCount = 1 shl primeFactors.size
        for (mask in 1..<subsetCount) {
            change += calculateSubsetContribution(mask, primeFactors, groupCnt)
        }
        return change
    }

    private fun calculateSubsetContribution(mask: Int, primeFactors: MutableList<Int?>, groupCnt: IntArray): Int {
        var product = 1
        var bits = 0
        for (k in primeFactors.indices) {
            if ((mask and (1 shl k)) == 0) {
                continue
            }
            bits++
            product *= primeFactors[k]!!
        }
        val contribution = groupCnt[product]
        return if (bits % 2 == 1) contribution else -contribution
    }

    private fun getFactor(number: Long): MutableList<Int?> {
        var n = number
        val primeFactors: MutableList<Int?> = ArrayList<Int?>()
        var i = 2
        while (i <= n / i) {
            if (n % i != 0L) {
                i++
                continue
            }
            primeFactors.add(i)
            while (n % i == 0L) {
                n /= i.toLong()
            }
            i++
        }
        if (n > 1) {
            primeFactors.add(n.toInt())
        }
        return primeFactors
    }
}
```