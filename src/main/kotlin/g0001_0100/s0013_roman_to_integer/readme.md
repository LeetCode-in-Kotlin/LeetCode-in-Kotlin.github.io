[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 13\. Roman to Integer

Easy

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

    Symbol  Value
     I       1
     V       5
     X       10
     L       50
     C       100
     D       500
     M       1000

For example, `2` is written as `II` in Roman numeral, just two one's added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

*   `I` can be placed before `V` (5) and `X` (10) to make 4 and 9.
*   `X` can be placed before `L` (50) and `C` (100) to make 40 and 90.
*   `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

**Example 1:**

**Input:** s = "III"

**Output:** 3 

**Example 2:**

**Input:** s = "IV"

**Output:** 4 

**Example 3:**

**Input:** s = "IX"

**Output:** 9 

**Example 4:**

**Input:** s = "LVIII"

**Output:** 58

**Explanation:** L = 50, V= 5, III = 3. 

**Example 5:**

**Input:** s = "MCMXCIV"

**Output:** 1994

**Explanation:** M = 1000, CM = 900, XC = 90 and IV = 4. 

**Constraints:**

*   `1 <= s.length <= 15`
*   `s` contains only the characters `('I', 'V', 'X', 'L', 'C', 'D', 'M')`.
*   It is **guaranteed** that `s` is a valid roman numeral in the range `[1, 3999]`.

## Solution

```kotlin
class Solution {
    fun romanToInt(s: String): Int {
        var x = 0
        var y: Char
        for (i in s.indices) {
            y = s[i]
            when (y) {
                'I' -> x = getX(s, x, i, 1, 'V', 'X')
                'V' -> x += 5
                'X' -> x = getX(s, x, i, 10, 'L', 'C')
                'L' -> x += 50
                'C' -> x = getX(s, x, i, 100, 'D', 'M')
                'D' -> x += 500
                'M' -> x += 1000
            }
        }
        return x
    }

    private fun getX(s: String, x: Int, i: Int, i2: Int, v: Char, x2: Char): Int {
        var localX = x
        if (i + 1 == s.length) {
            localX += i2
        } else if (s[i + 1] == v) {
            localX -= i2
        } else if (s[i + 1] == x2) {
            localX -= i2
        } else {
            localX += i2
        }
        return localX
    }
}
```