[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 670\. Maximum Swap

Medium

You are given an integer `num`. You can swap two digits at most once to get the maximum valued number.

Return _the maximum valued number you can get_.

**Example 1:**

**Input:** num = 2736

**Output:** 7236

**Explanation:** Swap the number 2 and the number 7.

**Example 2:**

**Input:** num = 9973

**Output:** 9973

**Explanation:** No swap.

**Constraints:**

*   <code>0 <= num <= 10<sup>8</sup></code>

## Solution

```kotlin
class Solution {
    fun maximumSwap(num: Int): Int {
        val chars = num.toString().toCharArray()
        for (i in chars.indices) {
            var j = chars.size - 1
            var indx = i
            var c = chars[i]
            while (j > i) {
                if (chars[j] > c) {
                    c = chars[j]
                    indx = j
                }
                j--
            }
            if (indx != i) {
                val temp = chars[i]
                chars[i] = chars[indx]
                chars[indx] = temp
                return String(chars).toInt()
            }
        }
        return num
    }
}
```