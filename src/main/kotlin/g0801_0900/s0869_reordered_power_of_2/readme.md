[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 869\. Reordered Power of 2

Medium

You are given an integer `n`. We reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return `true` _if and only if we can do this so that the resulting number is a power of two_.

**Example 1:**

**Input:** n = 1

**Output:** true

**Example 2:**

**Input:** n = 10

**Output:** false

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.pow

class Solution {
    fun reorderedPowerOf2(n: Int): Boolean {
        var i = 0
        while (2.0.pow(i.toDouble()) < n.toLong() * 10) {
            if (isValid(2.0.pow(i++.toDouble()).toInt().toString(), n.toString())) {
                return true
            }
        }
        return false
    }

    private fun isValid(a: String, b: String): Boolean {
        val m: MutableMap<Char, Int> = HashMap()
        val mTwo: MutableMap<Char, Int> = HashMap()
        for (c in a.toCharArray()) {
            m[c] = if (m.containsKey(c)) m[c]!! + 1 else 1
        }
        for (c in b.toCharArray()) {
            mTwo[c] = if (mTwo.containsKey(c)) mTwo[c]!! + 1 else 1
        }
        for (entry in mTwo.entries.iterator()) {
            if (!m.containsKey(entry.key) || entry.value != m[entry.key]) {
                return false
            }
        }
        return a[0] != '0' && m.size == mTwo.size
    }
}
```