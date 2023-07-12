[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2555\. Maximize Win From Two Segments

Medium

There are some prizes on the **X-axis**. You are given an integer array `prizePositions` that is **sorted in non-decreasing order**, where `prizePositions[i]` is the position of the <code>i<sup>th</sup></code> prize. There could be different prizes at the same position on the line. You are also given an integer `k`.

You are allowed to select two segments with integer endpoints. The length of each segment must be `k`. You will collect all prizes whose position falls within at least one of the two selected segments (including the endpoints of the segments). The two selected segments may intersect.

*   For example if `k = 2`, you can choose segments `[1, 3]` and `[2, 4]`, and you will win any prize i that satisfies `1 <= prizePositions[i] <= 3` or `2 <= prizePositions[i] <= 4`.

Return _the **maximum** number of prizes you can win if you choose the two segments optimally_.

**Example 1:**

**Input:** prizePositions = [1,1,2,2,3,3,5], k = 2

**Output:** 7

**Explanation:**

In this example, you can win all 7 prizes by selecting two segments [1, 3] and [3, 5].

**Example 2:**

**Input:** prizePositions = [1,2,3,4], k = 0

**Output:** 2

**Explanation:**

For this example, **one choice** for the segments is `[3, 3]` and `[4, 4],` and you will be able to get `2` prizes.

**Constraints:**

*   <code>1 <= prizePositions.length <= 10<sup>5</sup></code>
*   <code>1 <= prizePositions[i] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>9</sup></code>
*   `prizePositions` is sorted in non-decreasing order.

## Solution

```kotlin
class Solution {
    fun maximizeWin(p: IntArray, k: Int): Int {
        val n = p.size
        if (p[n - 1] - p[0] <= 2 * k) {
            return n
        }
        // segment ending in pre[j]
        val pre = IntArray(n)
        // segment starting in post[i]
        val post = IntArray(n)
        var i = 0
        var max = 0
        var j = 0
        while (j < n) {
            if (p[j] - p[i] > k) {
                i++
            }
            max = Math.max(max, j - i + 1)
            pre[j] = max
            j++
        }
        max = 0
        j = n - 1
        i = n - 1
        while (i >= 0) {
            if (p[j] - p[i] > k) {
                j--
            }
            max = Math.max(max, j - i + 1)
            post[i] = max
            i--
        }
        var res = 0
        var b = 0
        while (b + 1 < n) {
            res = Math.max(res, pre[b] + post[b + 1])
            b++
        }
        return res
    }
}
```