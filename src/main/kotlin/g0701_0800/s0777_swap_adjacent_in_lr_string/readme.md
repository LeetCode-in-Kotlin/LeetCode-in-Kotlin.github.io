[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 777\. Swap Adjacent in LR String

Medium

In a string composed of `'L'`, `'R'`, and `'X'` characters, like `"RXXLRXRXL"`, a move consists of either replacing one occurrence of `"XL"` with `"LX"`, or replacing one occurrence of `"RX"` with `"XR"`. Given the starting string `start` and the ending string `end`, return `True` if and only if there exists a sequence of moves to transform one string to the other.

**Example 1:**

**Input:** start = "RXXLRXRXL", end = "XRLXXRRLX"

**Output:** true

**Explanation:** We can transform start to end following these steps: 

RXXLRXRXL -> 

XRXLRXRXL -> 

XRLXRXRXL -> 

XRLXXRRXL -> 

XRLXXRRLX

**Example 2:**

**Input:** start = "X", end = "L"

**Output:** false

**Constraints:**

*   <code>1 <= start.length <= 10<sup>4</sup></code>
*   `start.length == end.length`
*   Both `start` and `end` will only consist of characters in `'L'`, `'R'`, and `'X'`.

## Solution

```kotlin
class Solution {
    fun canTransform(start: String, end: String): Boolean {
        var i1 = 0
        var i2 = 0
        val s1 = start.toCharArray()
        val s2 = end.toCharArray()
        while (i1 < s1.size && i2 < s2.size) {
            while (i1 < s1.size && s1[i1] == 'X') {
                i1++
            }
            while (i2 < s2.size && s2[i2] == 'X') {
                i2++
            }
            if (i1 == s1.size && i2 == s2.size) {
                return true
            }
            if (i1 == s1.size || i2 == s2.size) {
                return false
            }
            val c1 = s1[i1]
            val c2 = s2[i2]
            if (c1 != c2) {
                return false
            }
            if (c1 == 'L' && i1 < i2) {
                return false
            }
            if (c1 == 'R' && i1 > i2) {
                return false
            }
            i1++
            i2++
        }
        while (i1 < s1.size && start[i1] == 'X') {
            i1++
        }
        while (i2 < s2.size && end[i2] == 'X') {
            i2++
        }
        return i1 == s1.size && i2 == s2.size
    }
}
```