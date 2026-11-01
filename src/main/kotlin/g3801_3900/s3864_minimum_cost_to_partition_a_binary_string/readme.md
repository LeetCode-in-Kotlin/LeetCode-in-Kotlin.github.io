[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3864\. Minimum Cost to Partition a Binary String

Hard

You are given a binary string `s` and two integers `encCost` and `flatCost`.

For each index `i`, `s[i] = '1'` indicates that the <code>i<sup>th</sup></code> element is sensitive, and `s[i] = '0'` indicates that it is not.

The string must be partitioned into **segments**. Initially, the entire string forms a single segment.

For a segment of length `L` containing `X` sensitive elements:

*   If `X = 0`, the cost is `flatCost`.
*   If `X > 0`, the cost is `L * X * encCost`.

If a segment has **even length**, you may split it into **two contiguous segments** of **equal** length and the cost of this split is the **sum** of **costs** of the resulting segments.

Return an integer denoting the **minimum possible total cost** over all valid partitions.

**Example 1:**

**Input:** s = "1010", encCost = 2, flatCost = 1

**Output:** 6

**Explanation:**

*   The entire string `s = "1010"` has length 4 and contains 2 sensitive elements, giving a cost of `4 * 2 * 2 = 16`.
*   Since the length is even, it can be split into `"10"` and `"10"`. Each segment has length 2 and contains 1 sensitive element, so each costs `2 * 1 * 2 = 4`, giving a total of 8.
*   Splitting both segments into four single-character segments yields the segments `"1"`, `"0"`, `"1"`, and `"0"`. A segment containing `"1"` has length 1 and exactly one sensitive element, giving a cost of `1 * 1 * 2 = 2`, while a segment containing `"0"` has no sensitive elements and therefore costs `flatCost = 1`.
*   The total cost is thus `2 + 1 + 2 + 1 = 6`, which is the minimum possible total cost.

**Example 2:**

**Input:** s = "1010", encCost = 3, flatCost = 10

**Output:** 12

**Explanation:**

*   The entire string `s = "1010"` has length 4 and contains 2 sensitive elements, giving a cost of `4 * 2 * 3 = 24`.
*   Since the length is even, it can be split into two segments `"10"` and `"10"`.
*   Each segment has length 2 and contains one sensitive element, so each costs `2 * 1 * 3 = 6`, giving a total of 12, which is the minimum possible total cost.

**Example 3:**

**Input:** s = "00", encCost = 1, flatCost = 2

**Output:** 2

**Explanation:**

The string `s = "00"` has length 2 and contains no sensitive elements, so storing it as a single segment costs `flatCost = 2`, which is the minimum possible total cost.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of `'0'` and `'1'`.
*   <code>1 <= encCost, flatCost <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    private var encCost = 0
    private var flatCost = 0
    private lateinit var pre: IntArray

    fun minCost(s: String, encCost: Int, flatCost: Int): Long {
        pre = IntArray(s.length + 1)
        for (i in 1..<pre.size) {
            pre[i] = pre[i - 1] + (s[i - 1].code - '0'.code)
        }
        this.encCost = encCost
        this.flatCost = flatCost
        return helper(0, s.length)
    }

    private fun helper(l: Int, r: Int): Long {
        val gap = r - l
        val x = pre[r] - pre[l]
        if (x == 0) {
            return flatCost.toLong()
        }
        var cost = gap.toLong() * x * encCost
        if (gap % 2 == 0) {
            val mid = l + (r - l) / 2
            cost = min(cost, helper(l, mid) + helper(mid, r))
        }
        return cost
    }
}
```