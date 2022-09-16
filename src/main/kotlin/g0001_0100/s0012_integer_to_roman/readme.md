[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 12\. Integer to Roman

Medium

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

    Symbol   Value
     I        1
     V        5
     X        10
     L        50
     C        100
     D        500
     M        1000

For example, `2` is written as `II` in Roman numeral, just two one's added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

*   `I` can be placed before `V` (5) and `X` (10) to make 4 and 9.
*   `X` can be placed before `L` (50) and `C` (100) to make 40 and 90.
*   `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral.

**Example 1:**

**Input:** num = 3

**Output:** "III" 

**Example 2:**

**Input:** num = 4

**Output:** "IV" 

**Example 3:**

**Input:** num = 9

**Output:** "IX" 

**Example 4:**

**Input:** num = 58

**Output:** "LVIII"

**Explanation:** L = 50, V = 5, III = 3. 

**Example 5:**

**Input:** num = 1994

**Output:** "MCMXCIV"

**Explanation:** M = 1000, CM = 900, XC = 90 and IV = 4. 

**Constraints:**

*   `1 <= num <= 3999`

## Solution

```kotlin
class Solution {
    fun intToRoman(num: Int): String? {
        var localNum = num
        val sb = StringBuilder()
        val m = 1000
        val c = 100
        val x = 10
        val i = 1
        localNum = numerals(sb, localNum, m, ' ', ' ', 'M')
        localNum = numerals(sb, localNum, c, 'M', 'D', 'C')
        localNum = numerals(sb, localNum, x, 'C', 'L', 'X')
        numerals(sb, localNum, i, 'X', 'V', 'I')
        return sb.toString()
    }

    private fun numerals(sb: StringBuilder, num: Int, one: Int, cTen: Char, cFive: Char, cOne: Char): Int {
        val div = num / one
        when (div) {
            9 -> {
                sb.append(cOne)
                sb.append(cTen)
            }
            8 -> {
                sb.append(cFive)
                sb.append(cOne)
                sb.append(cOne)
                sb.append(cOne)
            }
            7 -> {
                sb.append(cFive)
                sb.append(cOne)
                sb.append(cOne)
            }
            6 -> {
                sb.append(cFive)
                sb.append(cOne)
            }
            5 -> sb.append(cFive)
            4 -> {
                sb.append(cOne)
                sb.append(cFive)
            }
            3 -> {
                sb.append(cOne)
                sb.append(cOne)
                sb.append(cOne)
            }
            2 -> {
                sb.append(cOne)
                sb.append(cOne)
            }
            1 -> sb.append(cOne)
        }
        return num - div * one
    }
}
```