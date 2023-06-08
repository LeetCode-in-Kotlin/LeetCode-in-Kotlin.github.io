[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1324\. Print Words Vertically

Medium

Given a string `s`. Return all the words vertically in the same order in which they appear in `s`.  
Words are returned as a list of strings, complete with spaces when is necessary. (Trailing spaces are not allowed).  
Each word would be put on only one column and that in one column there will be only one word.

**Example 1:**

**Input:** s = "HOW ARE YOU"

**Output:** ["HAY","ORO","WEU"]

**Explanation:** Each word is printed vertically. 

"HAY"

"ORO"

"WEU"

**Example 2:**

**Input:** s = "TO BE OR NOT TO BE"

**Output:** ["TBONTB","OEROOE"," T"]

**Explanation:** Trailing spaces is not allowed. 

"TBONTB" 

"OEROOE" 

" T"

**Example 3:**

**Input:** s = "CONTEST IS COMING"

**Output:** ["CIC","OSO","N M","T I","E N","S G","T"]

**Constraints:**

*   `1 <= s.length <= 200`
*   `s` contains only upper case English letters.
*   It's guaranteed that there is only one space between 2 words.

## Solution

```kotlin
class Solution {
    fun printVertically(s: String): List<String> {
        val words = s.split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        var columnMax = 0
        for (word in words) {
            columnMax = Math.max(columnMax, word.length)
        }
        val matrix = Array(words.size) { CharArray(columnMax) }
        for (i in words.indices) {
            var j = 0
            while (j < words[i].length) {
                matrix[i][j] = words[i][j]
                j++
            }
            while (j < columnMax) {
                matrix[i][j++] = '#'
            }
        }
        val result: MutableList<String> = ArrayList()
        for (j in 0 until columnMax) {
            val sb = StringBuilder()
            for (chars in matrix) {
                if (chars[j] != '#') {
                    sb.append(chars[j])
                } else {
                    sb.append(' ')
                }
            }
            val str = sb.toString()
            var k = str.length - 1
            while (k >= 0 && str[k] == ' ') {
                k--
            }
            result.add(str.substring(0, k + 1))
            sb.setLength(0)
        }
        return result
    }
}
```