[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1732\. Find the Highest Altitude

Easy

There is a biker going on a road trip. The road trip consists of `n + 1` points at different altitudes. The biker starts his trip on point `0` with altitude equal `0`.

You are given an integer array `gain` of length `n` where `gain[i]` is the **net gain in altitude** between points `i` and `i + 1` for all (`0 <= i < n)`. Return _the **highest altitude** of a point._

**Example 1:**

**Input:** gain = [-5,1,5,0,-7]

**Output:** 1

**Explanation:** The altitudes are [0,-5,-4,1,1,-6]. The highest is 1.

**Example 2:**

**Input:** gain = [-4,-3,-2,-1,4,3,2]

**Output:** 0

**Explanation:** The altitudes are [0,-4,-7,-9,-10,-6,-3,-1]. The highest is 0.

**Constraints:**

*   `n == gain.length`
*   `1 <= n <= 100`
*   `-100 <= gain[i] <= 100`

## Solution

```kotlin
class Solution {
    fun largestAltitude(gain: IntArray): Int {
        var max = 0
        val altitudes = IntArray(gain.size + 1)
        for (i in gain.indices) {
            altitudes[i + 1] = altitudes[i] + gain[i]
            max = Math.max(max, altitudes[i + 1])
        }
        return max
    }
}
```