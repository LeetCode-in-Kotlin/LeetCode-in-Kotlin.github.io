[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3180\. Maximum Total Reward Using Operations I

Medium

You are given an integer array `rewardValues` of length `n`, representing the values of rewards.

Initially, your total reward `x` is 0, and all indices are **unmarked**. You are allowed to perform the following operation **any** number of times:

*   Choose an **unmarked** index `i` from the range `[0, n - 1]`.
*   If `rewardValues[i]` is **greater** than your current total reward `x`, then add `rewardValues[i]` to `x` (i.e., `x = x + rewardValues[i]`), and **mark** the index `i`.

Return an integer denoting the **maximum** _total reward_ you can collect by performing the operations optimally.

**Example 1:**

**Input:** rewardValues = [1,1,3,3]

**Output:** 4

**Explanation:**

During the operations, we can choose to mark the indices 0 and 2 in order, and the total reward will be 4, which is the maximum.

**Example 2:**

**Input:** rewardValues = [1,6,4,3,2]

**Output:** 11

**Explanation:**

Mark the indices 0, 2, and 1 in order. The total reward will then be 11, which is the maximum.

**Constraints:**

*   `1 <= rewardValues.length <= 2000`
*   `1 <= rewardValues[i] <= 2000`

## Solution

```kotlin
class Solution {
    private fun sortedSet(values: IntArray): IntArray {
        var max = 0
        for (x in values) {
            if (x > max) {
                max = x
            }
        }
        val set = BooleanArray(max + 1)
        var n = 0
        for (x in values) {
            if (!set[x]) {
                set[x] = true
                n++
            }
        }
        val result = IntArray(n)
        for (x in max downTo 1) {
            if (set[x]) {
                result[--n] = x
            }
        }
        return result
    }

    fun maxTotalReward(rewardValues: IntArray): Int {
        var rewardValues = rewardValues
        rewardValues = sortedSet(rewardValues)
        val n = rewardValues.size
        val max = rewardValues[n - 1]
        val isSumPossible = BooleanArray(max)
        isSumPossible[0] = true
        var maxSum = 0
        var last = 1
        for (sum in rewardValues[0] until max) {
            while (last < n && rewardValues[last] <= sum) {
                last++
            }
            val s2 = sum / 2
            for (i in last - 1 downTo 0) {
                val x = rewardValues[i]
                if (x <= s2) {
                    break
                }
                if (isSumPossible[sum - x]) {
                    isSumPossible[sum] = true
                    maxSum = sum
                    break
                }
            }
        }
        return maxSum + max
    }
}
```