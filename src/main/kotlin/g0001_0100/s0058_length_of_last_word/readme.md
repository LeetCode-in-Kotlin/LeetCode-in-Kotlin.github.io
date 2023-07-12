[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 58\. Length of Last Word

Easy

Given a string `s` consisting of words and spaces, return _the length of the **last** word in the string._

A **word** is a maximal substring consisting of non-space characters only.

**Example 1:**

**Input:** s = "Hello World"

**Output:** 5

**Explanation:** The last word is "World" with length 5.

**Example 2:**

**Input:** s = " fly me to the moon "

**Output:** 4

**Explanation:** The last word is "moon" with length 4.

**Example 3:**

**Input:** s = "luffy is still joyboy"

**Output:** 6

**Explanation:** The last word is "joyboy" with length 6.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of only English letters and spaces `' '`.
*   There will be at least one word in `s`.

## Solution

```kotlin
class Solution {
    fun lengthOfLastWord(s: String): Int {
        var len = 0
        for (i in s.length - 1 downTo 0) {
            val ch = s[i]
            if (ch == ' ' && len > 0) {
                break
            } else if (ch != ' ') {
                len++
            }
        }
        return len
    }
}
```