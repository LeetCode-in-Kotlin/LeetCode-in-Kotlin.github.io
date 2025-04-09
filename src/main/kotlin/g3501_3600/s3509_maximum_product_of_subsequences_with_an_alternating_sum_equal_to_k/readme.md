[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3509\. Maximum Product of Subsequences With an Alternating Sum Equal to K

Hard

You are given an integer array `nums` and two integers, `k` and `limit`. Your task is to find a non-empty ****subsequences**** of `nums` that:

*   Has an **alternating sum** equal to `k`.
*   **Maximizes** the product of all its numbers _without the product exceeding_ `limit`.

Return the _product_ of the numbers in such a subsequence. If no subsequence satisfies the requirements, return -1.

The **alternating sum** of a **0-indexed** array is defined as the **sum** of the elements at **even** indices **minus** the **sum** of the elements at **odd** indices.

**Example 1:**

**Input:** nums = [1,2,3], k = 2, limit = 10

**Output:** 6

**Explanation:**

The subsequences with an alternating sum of 2 are:

*   `[1, 2, 3]`
    *   Alternating Sum: `1 - 2 + 3 = 2`
    *   Product: `1 * 2 * 3 = 6`
*   `[2]`
    *   Alternating Sum: 2
    *   Product: 2

The maximum product within the limit is 6.

**Example 2:**

**Input:** nums = [0,2,3], k = -5, limit = 12

**Output:** \-1

**Explanation:**

A subsequence with an alternating sum of exactly -5 does not exist.

**Example 3:**

**Input:** nums = [2,2,3,3], k = 0, limit = 9

**Output:** 9

**Explanation:**

The subsequences with an alternating sum of 0 are:

*   `[2, 2]`
    *   Alternating Sum: `2 - 2 = 0`
    *   Product: `2 * 2 = 4`
*   `[3, 3]`
    *   Alternating Sum: `3 - 3 = 0`
    *   Product: `3 * 3 = 9`
*   `[2, 2, 3, 3]`
    *   Alternating Sum: `2 - 2 + 3 - 3 = 0`
    *   Product: `2 * 2 * 3 * 3 = 36`

The subsequence `[2, 2, 3, 3]` has the greatest product with an alternating sum equal to `k`, but `36 > 9`. The next greatest product is 9, which is within the limit.

**Constraints:**

*   `1 <= nums.length <= 150`
*   `0 <= nums[i] <= 12`
*   <code>-10<sup>5</sup> <= k <= 10<sup>5</sup></code>
*   `1 <= limit <= 5000`

## Solution

```kotlin
import java.util.BitSet
import kotlin.math.max

class Solution {
    internal class StateKey(var prod: Int, var parity: Int) {
        override fun hashCode(): Int {
            return prod * 31 + parity
        }

        override fun equals(other: Any?): Boolean {
            if (other !is StateKey) {
                return false
            }
            return this.prod == other.prod && this.parity == other.parity
        }
    }

    fun maxProduct(nums: IntArray, k: Int, limit: Int): Int {
        val melkarvothi = nums.clone()
        val offset = 1000
        val size = 2100
        val dp: MutableMap<StateKey, BitSet> = HashMap<StateKey, BitSet>()
        for (x in melkarvothi) {
            val newStates: MutableMap<StateKey, BitSet> = HashMap<StateKey, BitSet>()
            for (entry in dp.entries) {
                val key: StateKey = entry.key
                val currentProd = key.prod
                val newProd: Int
                if (x == 0) {
                    newProd = 0
                } else {
                    if (currentProd == 0) {
                        newProd = 0
                    } else if (currentProd == -1) {
                        newProd = -1
                    } else {
                        val mult = currentProd.toLong() * x
                        if (mult > limit) {
                            newProd = -1
                        } else {
                            newProd = mult.toInt()
                        }
                    }
                }
                val newParity = 1 - key.parity
                val bs: BitSet = entry.value
                val shifted: BitSet
                if (key.parity == 0) {
                    shifted = shift(bs, x, size)
                } else {
                    shifted = shift(bs, -x, size)
                }
                val newKey = StateKey(newProd, newParity)
                var current = newStates[newKey]
                if (current == null) {
                    current = BitSet(size)
                    newStates.put(newKey, current)
                }
                current.or(shifted)
            }
            if (x == 0 || x <= limit) {
                val parityStart = 1
                val newKey = StateKey(x, parityStart)
                var bs = newStates[newKey]
                if (bs == null) {
                    bs = BitSet(size)
                    newStates.put(newKey, bs)
                }
                val pos = x + offset
                if (pos >= 0 && pos < size) {
                    bs.set(pos)
                }
            }
            for (entry in newStates.entries) {
                val key = entry.key
                val newBS: BitSet = entry.value
                val oldBS = dp[key]
                if (oldBS == null) {
                    dp.put(key, newBS)
                } else {
                    oldBS.or(newBS)
                }
            }
        }
        var answer = -1
        val targetIdx = k + offset
        for (entry in dp.entries) {
            val key: StateKey = entry.key
            if (key.prod == -1) {
                continue
            }
            val bs: BitSet = entry.value
            if (targetIdx >= 0 && targetIdx < size && bs[targetIdx]) {
                answer = max(answer, key.prod)
            }
        }
        return answer
    }

    private fun shift(bs: BitSet, shiftVal: Int, size: Int): BitSet {
        val res = BitSet(size)
        if (shiftVal >= 0) {
            var i = bs.nextSetBit(0)
            while (i >= 0) {
                val newIdx = i + shiftVal
                if (newIdx < size) {
                    res.set(newIdx)
                }
                i = bs.nextSetBit(i + 1)
            }
        } else {
            val shiftRight = -shiftVal
            var i = bs.nextSetBit(0)
            while (i >= 0) {
                val newIdx = i - shiftRight
                if (newIdx >= 0) {
                    res.set(newIdx)
                }
                i = bs.nextSetBit(i + 1)
            }
        }
        return res
    }
}
```