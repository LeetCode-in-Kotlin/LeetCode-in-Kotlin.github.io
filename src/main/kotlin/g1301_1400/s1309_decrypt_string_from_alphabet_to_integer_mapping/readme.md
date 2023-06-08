[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1309\. Decrypt String from Alphabet to Integer Mapping

Easy

You are given a string `s` formed by digits and `'#'`. We want to map `s` to English lowercase characters as follows:

*   Characters (`'a'` to `'i')` are represented by (`'1'` to `'9'`) respectively.
*   Characters (`'j'` to `'z')` are represented by (`'10#'` to `'26#'`) respectively.

Return _the string formed after mapping_.

The test cases are generated so that a unique mapping will always exist.

**Example 1:**

**Input:** s = "10#11#12"

**Output:** "jkab"

**Explanation:** "j" -> "10#" , "k" -> "11#" , "a" -> "1" , "b" -> "2".

**Example 2:**

**Input:** s = "1326#"

**Output:** "acz"

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of digits and the `'#'` letter.
*   `s` will be a valid string such that mapping is always possible.

## Solution

```kotlin
class Solution {
    fun freqAlphabets(s: String): String {
        val map: MutableMap<String, String> = HashMap()
        map["1"] = "a"
        map["2"] = "b"
        map["3"] = "c"
        map["4"] = "d"
        map["5"] = "e"
        map["6"] = "f"
        map["7"] = "g"
        map["8"] = "h"
        map["9"] = "i"
        map["10#"] = "j"
        map["11#"] = "k"
        map["12#"] = "l"
        map["13#"] = "m"
        map["14#"] = "n"
        map["15#"] = "o"
        map["16#"] = "p"
        map["17#"] = "q"
        map["18#"] = "r"
        map["19#"] = "s"
        map["20#"] = "t"
        map["21#"] = "u"
        map["22#"] = "v"
        map["23#"] = "w"
        map["24#"] = "x"
        map["25#"] = "y"
        map["26#"] = "z"
        val sb = StringBuilder()
        var i = 0
        while (i < s.length) {
            if ((("" + s[i]).toInt() == 1 || ("" + s[i]).toInt() == 2) &&
                i + 1 < s.length && i + 2 < s.length &&
                s[i + 2] == '#'
            ) {
                sb.append(map[s.substring(i, i + 3)])
                i += 3
            } else {
                sb.append(map["" + s[i]])
                i++
            }
        }
        return sb.toString()
    }
}
```