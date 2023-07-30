[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2434\. Using a Robot to Print the Lexicographically Smallest String

Medium

You are given a string `s` and a robot that currently holds an empty string `t`. Apply one of the following operations until `s` and `t` **are both empty**:

*   Remove the **first** character of a string `s` and give it to the robot. The robot will append this character to the string `t`.
*   Remove the **last** character of a string `t` and give it to the robot. The robot will write this character on paper.

Return _the lexicographically smallest string that can be written on the paper._

**Example 1:**

**Input:** s = "zza"

**Output:** "azz"

**Explanation:** Let p denote the written string. 

Initially p="", s="zza", t="". 

Perform first operation three times p="", s="", t="zza". 

Perform second operation three times p="azz", s="", t="".

**Example 2:**

**Input:** s = "bac"

**Output:** "abc"

**Explanation:** Let p denote the written string. 

Perform first operation twice p="", s="c", t="ba".

Perform second operation twice p="ab", s="c", t="". 

Perform first operation p="ab", s="", t="c". 

Perform second operation p="abc", s="", t="".

**Example 3:**

**Input:** s = "bdda"

**Output:** "addb"

**Explanation:** Let p denote the written string. 

Initially p="", s="bdda", t="". 

Perform first operation four times p="", s="", t="bdda". 

Perform second operation four times p="addb", s="", t="".

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only English lowercase letters.

## Solution

```kotlin
class Solution {
    fun robotWithString(s: String): String {
        val n = s.length
        val c = s.toCharArray()
        val next = CharArray(n + 1)
        next[n] = ('z'.code + 1).toChar()
        for (i in n - 1 downTo 0) {
            next[i] = ('a'.code + (c[i].code - 'a'.code).coerceAtMost(next[i + 1].code - 'a'.code)).toChar()
        }
        val stack = CharArray(n)
        var j = 0
        var k = 0
        for (i in 0 until n) {
            if (c[i] == next[i]) {
                c[j++] = c[i]
                while (k > 0 && stack[k - 1] <= next[i + 1]) {
                    c[j++] = stack[--k]
                }
            } else {
                stack[k++] = c[i]
            }
        }
        while (k > 0) {
            c[j++] = stack[--k]
        }
        return String(c)
    }
}
```