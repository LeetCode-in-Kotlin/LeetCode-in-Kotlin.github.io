[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 844\. Backspace String Compare

Easy

Given two strings `s` and `t`, return `true` _if they are equal when both are typed into empty text editors_. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

**Example 1:**

**Input:** s = "ab#c", t = "ad#c"

**Output:** true

**Explanation:** Both s and t become "ac".

**Example 2:**

**Input:** s = "ab##", t = "c#d#"

**Output:** true

**Explanation:** Both s and t become "".

**Example 3:**

**Input:** s = "a#c", t = "b"

**Output:** false

**Explanation:** s becomes "c" while t becomes "b".

**Constraints:**

*   `1 <= s.length, t.length <= 200`
*   `s` and `t` only contain lowercase letters and `'#'` characters.

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?

## Solution

```kotlin
class Solution {
    fun backspaceCompare(s: String, t: String): Boolean {
        return cmprStr(s) == cmprStr(t)
    }

    private fun cmprStr(str: String): String {
        val res = StringBuilder()
        var count = 0
        for (i in str.length - 1 downTo 0) {
            val currChar = str[i]
            if (currChar == '#') {
                count++
            } else {
                if (count > 0) {
                    count--
                } else {
                    res.append(currChar)
                }
            }
        }
        return res.toString()
    }
}
```