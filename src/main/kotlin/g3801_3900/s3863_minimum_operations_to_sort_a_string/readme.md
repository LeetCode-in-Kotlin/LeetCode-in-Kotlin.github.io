[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3863\. Minimum Operations to Sort a String

Medium

You are given a string `s` consisting of lowercase English letters.

In one operation, you can select any **substring** of `s` that is **not** the entire string and **sort** it in **non-descending alphabetical** order.

Return the **minimum** number of operations required to make `s` sorted in **non-descending** order. If it is not possible, return -1.

**Example 1:**

**Input:** s = "dog"

**Output:** 1

**Explanation:**

*   Sort substring `"og"` to `"go"`.
*   Now, `s = "dgo"`, which is sorted in ascending order. Thus, the answer is 1.

**Example 2:**

**Input:** s = "card"

**Output:** 2

**Explanation:**

*   Sort substring `"car"` to `"acr"`, so `s = "acrd"`.
*   Sort substring `"rd"` to `"dr"`, making `s = "acdr"`, which is sorted in ascending order. Thus, the answer is 2.

**Example 3:**

**Input:** s = "gf"

**Output:** \-1

**Explanation:**

*   It is impossible to sort `s` under the given constraints. Thus, the answer is -1.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minOperations(s: String): Int {
        val n = s.length
        if (n == 1) {
            return 0
        }
        if (n == 2) {
            return if (s[0] > s[1]) -1 else 0
        }
        var min = 'z'
        var max = 'a'
        val first = s[0]
        val last = s[n - 1]
        var prev = 'a'
        val cnt = IntArray(26)
        var sorted = true
        for (c in s.toCharArray()) {
            sorted = sorted and (prev <= c)
            min = min(min.code, c.code).toChar()
            max = max(max.code, c.code).toChar()
            prev = c
            cnt[c.code - 'a'.code]++
        }
        if (sorted) {
            return 0
        }
        return calculateOperations(first, last, min, max, cnt)
    }

    private fun calculateOperations(first: Char, last: Char, min: Char, max: Char, cnt: IntArray): Int {
        if (first == min || last == max) {
            return 1
        }
        if (first != max || last != min) {
            return 2
        }
        return if (cnt[max.code - 'a'.code] > 1 || cnt[min.code - 'a'.code] > 1) 2 else 3
    }
}
```