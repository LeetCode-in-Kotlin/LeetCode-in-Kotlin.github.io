[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1012\. Numbers With Repeated Digits

Hard

Given an integer `n`, return _the number of positive integers in the range_ `[1, n]` _that have **at least one** repeated digit_.

**Example 1:**

**Input:** n = 20

**Output:** 1

**Explanation:** The only positive number (<= 20) with at least 1 repeated digit is 11.

**Example 2:**

**Input:** n = 100

**Output:** 10

**Explanation:** The positive numbers (<= 100) with atleast 1 repeated digit are 11, 22, 33, 44, 55, 66, 77, 88, 99, and 100.

**Example 3:**

**Input:** n = 1000

**Output:** 262

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.pow

class Solution {
    private var noRepeatCount = 0
    fun numDupDigitsAtMostN(n: Int): Int {
        val nStrLength = n.toString().length
        val allNineLength: Int = if (n < 0 || nStrLength < 2) {
            return 0
        } else if (10.0.pow(nStrLength.toDouble()) - 1 == n.toDouble()) {
            nStrLength
        } else {
            nStrLength - 1
        }
        for (numberOfDigits in 1..allNineLength) {
            noRepeatCount += calcNumberOfNoRepeat(numberOfDigits)
        }
        if (10.0.pow(nStrLength.toDouble()) - 1 > n) {
            val mutations = 10
            val hs: HashSet<Int> = HashSet()
            for (index1 in 0 until nStrLength) {
                var noRepeatCountLocal = 0
                hs.clear()
                for (index2 in 0 until nStrLength) {
                    val index2Digit = (
                        n /
                            10.0.pow(n.toString().length - (index2 + 1.0)) %
                            10
                        ).toInt()
                    if (index2 < index1) {
                        if (hs.contains(index2Digit)) {
                            noRepeatCountLocal = 0
                            break
                        } else {
                            hs.add(index2Digit)
                        }
                    } else if (index2 == index1) {
                        noRepeatCountLocal = if (index2 == 0) {
                            index2Digit - 1
                        } else {
                            var inIndex2Range = 0
                            for (j in hs) {
                                if (index2 < nStrLength - 1 && j <= index2Digit - 1 ||
                                    index2 == nStrLength - 1 && j <= index2Digit
                                ) {
                                    inIndex2Range++
                                }
                            }
                            if (index2 == nStrLength - 1) {
                                index2Digit + 1 - inIndex2Range
                            } else {
                                index2Digit - inIndex2Range
                            }
                        }
                    } else {
                        noRepeatCountLocal *= mutations - index2
                    }
                }
                if (noRepeatCountLocal > 0) {
                    noRepeatCount += noRepeatCountLocal
                }
            }
        }
        return n - noRepeatCount
    }

    private fun calcNumberOfNoRepeat(numberOfDigits: Int): Int {
        var repeatCount = 0
        var mutations = 9
        for (i in 0 until numberOfDigits) {
            if (i == 0) {
                repeatCount = mutations
            } else {
                repeatCount *= mutations--
            }
        }
        return repeatCount
    }
}
```