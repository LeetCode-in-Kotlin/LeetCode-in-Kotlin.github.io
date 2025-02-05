[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3443\. Maximum Manhattan Distance After K Changes

Medium

You are given a string `s` consisting of the characters `'N'`, `'S'`, `'E'`, and `'W'`, where `s[i]` indicates movements in an infinite grid:

*   `'N'` : Move north by 1 unit.
*   `'S'` : Move south by 1 unit.
*   `'E'` : Move east by 1 unit.
*   `'W'` : Move west by 1 unit.

Initially, you are at the origin `(0, 0)`. You can change **at most** `k` characters to any of the four directions.

Find the **maximum** **Manhattan distance** from the origin that can be achieved **at any time** while performing the movements **in order**.

The **Manhattan Distance** between two cells <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and <code>(x<sub>j</sub>, y<sub>j</sub>)</code> is <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.

**Example 1:**

**Input:** s = "NWSE", k = 1

**Output:** 3

**Explanation:**

Change `s[2]` from `'S'` to `'N'`. The string `s` becomes `"NWNE"`.

| Movement         | Position (x, y) | Manhattan Distance | Maximum |
|-----------------|----------------|--------------------|---------|
| s[0] == 'N'    | (0, 1)         | 0 + 1 = 1         | 1       |
| s[1] == 'W'    | (-1, 1)        | 1 + 1 = 2         | 2       |
| s[2] == 'N'    | (-1, 2)        | 1 + 2 = 3         | 3       |
| s[3] == 'E'    | (0, 2)         | 0 + 2 = 2         | 3       |

The maximum Manhattan distance from the origin that can be achieved is 3. Hence, 3 is the output.

**Example 2:**

**Input:** s = "NSWWEW", k = 3

**Output:** 6

**Explanation:**

Change `s[1]` from `'S'` to `'N'`, and `s[4]` from `'E'` to `'W'`. The string `s` becomes `"NNWWWW"`.

The maximum Manhattan distance from the origin that can be achieved is 6. Hence, 6 is the output.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `0 <= k <= s.length`
*   `s` consists of only `'N'`, `'S'`, `'E'`, and `'W'`.

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxDistance(s: String, k: Int): Int {
        var north = 0
        var south = 0
        var west = 0
        var east = 0
        var result = 0
        for (c in s.toCharArray()) {
            if (c == 'N') {
                north++
            } else if (c == 'S') {
                south++
            } else if (c == 'E') {
                east++
            } else if (c == 'W') {
                west++
            }
            val hMax = max(north, south)
            val vMax = max(west, east)
            val hMin = min(north, south)
            val vMin = min(west, east)
            if (hMin + vMin >= k) {
                val curr = hMax + vMax + k - (hMin + vMin - k)
                result = max(result, curr)
            } else {
                val curr = hMax + vMax + hMin + vMin
                result = max(result, curr)
            }
        }
        return result
    }
}
```