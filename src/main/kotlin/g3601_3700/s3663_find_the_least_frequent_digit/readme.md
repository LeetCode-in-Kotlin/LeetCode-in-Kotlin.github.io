[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3663\. Find The Least Frequent Digit

Easy

Given an integer `n`, find the digit that occurs **least** frequently in its decimal representation. If multiple digits have the same frequency, choose the **smallest** digit.

Return the chosen digit as an integer.

The **frequency** of a digit `x` is the number of times it appears in the decimal representation of `n`.

**Example 1:**

**Input:** n = 1553322

**Output:** 1

**Explanation:**

The least frequent digit in `n` is 1, which appears only once. All other digits appear twice.

**Example 2:**

**Input:** n = 723344511

**Output:** 2

**Explanation:**

The least frequent digits in `n` are 7, 2, and 5; each appears only once.

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun getLeastFrequentDigit(n: Int): Int {
        val freq = IntArray(10)
        val numStr = n.toString()
        for (c in numStr.toCharArray()) {
            freq[c.code - '0'.code]++
        }
        var minFreq = Int.Companion.MAX_VALUE
        var result = -1
        for (d in 0..9) {
            if (freq[d] == 0) {
                continue
            }
            if (freq[d] < minFreq) {
                minFreq = freq[d]
                result = d
            } else if (freq[d] == minFreq && d < result) {
                result = d
            }
        }
        return result
    }
}
```