[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3357\. Minimize the Maximum Adjacent Element Difference

Hard

You are given an array of integers `nums`. Some values in `nums` are **missing** and are denoted by -1.

You can choose a pair of **positive** integers `(x, y)` **exactly once** and replace each **missing** element with _either_ `x` or `y`.

You need to **minimize** the **maximum** **absolute difference** between _adjacent_ elements of `nums` after replacements.

Return the **minimum** possible difference.

**Example 1:**

**Input:** nums = [1,2,-1,10,8]

**Output:** 4

**Explanation:**

By choosing the pair as `(6, 7)`, nums can be changed to `[1, 2, 6, 10, 8]`.

The absolute differences between adjacent elements are:

*   `|1 - 2| == 1`
*   `|2 - 6| == 4`
*   `|6 - 10| == 4`
*   `|10 - 8| == 2`

**Example 2:**

**Input:** nums = [-1,-1,-1]

**Output:** 0

**Explanation:**

By choosing the pair as `(4, 4)`, nums can be changed to `[4, 4, 4]`.

**Example 3:**

**Input:** nums = [-1,10,-1,8]

**Output:** 1

**Explanation:**

By choosing the pair as `(11, 9)`, nums can be changed to `[11, 10, 9, 8]`.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either -1 or in the range <code>[1, 10<sup>9</sup>]</code>.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minDifference(nums: IntArray): Int {
        val n = nums.size
        var maxAdj = 0
        var mina = Int.Companion.MAX_VALUE
        var maxb = Int.Companion.MIN_VALUE
        for (i in 0..<n - 1) {
            val a = nums[i]
            val b = nums[i + 1]
            if (a > 0 && b > 0) {
                maxAdj = max(maxAdj, abs((a - b)))
            } else if (a > 0 || b > 0) {
                mina = min(mina, max(a, b))
                maxb = max(maxb, max(a, b))
            }
        }
        var res = 0
        for (i in 0..<n) {
            if ((i > 0 && nums[i - 1] == -1) || nums[i] > 0) {
                continue
            }
            var j = i
            while (j < n && nums[j] == -1) {
                j++
            }
            var a = Int.Companion.MAX_VALUE
            var b = Int.Companion.MIN_VALUE
            if (i > 0) {
                a = min(a, nums[i - 1])
                b = max(b, nums[i - 1])
            }
            if (j < n) {
                a = min(a, nums[j])
                b = max(b, nums[j])
            }
            if (a <= b) {
                if (j - i == 1) {
                    res = max(res, min((maxb - a), (b - mina)))
                } else {
                    res = max(
                        res,
                        min(
                            maxb - a,
                            min(b - mina, (maxb - mina + 2) / 3 * 2),
                        ),
                    )
                }
            }
        }
        return max(maxAdj, (res + 1) / 2)
    }
}
```