[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 956\. Tallest Billboard

Hard

You are installing a billboard and want it to have the largest height. The billboard will have two steel supports, one on each side. Each steel support must be an equal height.

You are given a collection of `rods` that can be welded together. For example, if you have rods of lengths `1`, `2`, and `3`, you can weld them together to make a support of length `6`.

Return _the largest possible height of your billboard installation_. If you cannot support the billboard, return `0`.

**Example 1:**

**Input:** rods = [1,2,3,6]

**Output:** 6

**Explanation:** We have two disjoint subsets {1,2,3} and {6}, which have the same sum = 6.

**Example 2:**

**Input:** rods = [1,2,3,4,5,6]

**Output:** 10

**Explanation:** We have two disjoint subsets {2,3,5} and {4,6}, which have the same sum = 10.

**Example 3:**

**Input:** rods = [1,2]

**Output:** 0

**Explanation:** The billboard cannot be supported, so we return 0.

**Constraints:**

*   `1 <= rods.length <= 20`
*   `1 <= rods[i] <= 1000`
*   `sum(rods[i]) <= 5000`

## Solution

```kotlin
class Solution {
    fun tallestBillboard(rods: IntArray): Int {
        var maxDiff = 0
        for (rod in rods) {
            maxDiff += rod
        }
        val dp = IntArray(maxDiff + 1)
        dp.fill(-1)
        dp[0] = 0
        for (l in rods) {
            val dpOld = IntArray(maxDiff + 1)
            System.arraycopy(dp, 0, dpOld, 0, maxDiff + 1)
            for (diff in 0 until maxDiff + 1) {
                if (dpOld[diff] == -1) {
                    continue
                }
                if (diff + l <= maxDiff) {
                    dp[diff + l] = dp[diff + l].coerceAtLeast(dpOld[diff] + l)
                }
                if (l - diff >= 0) {
                    dp[l - diff] = dp[l - diff].coerceAtLeast(l + dpOld[diff] - diff)
                } else {
                    dp[diff - l] = dp[diff - l].coerceAtLeast(dpOld[diff])
                }
            }
        }
        return dp[0]
    }
}
```