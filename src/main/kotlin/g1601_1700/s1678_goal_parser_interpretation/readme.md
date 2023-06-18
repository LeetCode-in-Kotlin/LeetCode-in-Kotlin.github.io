[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1678\. Goal Parser Interpretation

Easy

You own a **Goal Parser** that can interpret a string `command`. The `command` consists of an alphabet of `"G"`, `"()"` and/or `"(al)"` in some order. The Goal Parser will interpret `"G"` as the string `"G"`, `"()"` as the string `"o"`, and `"(al)"` as the string `"al"`. The interpreted strings are then concatenated in the original order.

Given the string `command`, return _the **Goal Parser**'s interpretation of_ `command`.

**Example 1:**

**Input:** command = "G()(al)"

**Output:** "Goal"

**Explanation:** The Goal Parser interprets the command as follows:

G -> G

() -> o

(al) -> al

The final concatenated result is "Goal".

**Example 2:**

**Input:** command = "G()()()()(al)"

**Output:** "Gooooal"

**Example 3:**

**Input:** command = "(al)G(al)()()G"

**Output:** "alGalooG"

**Constraints:**

*   `1 <= command.length <= 100`
*   `command` consists of `"G"`, `"()"`, and/or `"(al)"` in some order.

## Solution

```kotlin
class Solution {
    fun interpret(command: String): String {
        val sb = StringBuilder()
        var i = 0
        while (i < command.length) {
            if (command[i] == '(' && command[i + 1] == ')') {
                sb.append("o")
                i++
            } else if ((command[i] != '(' || command[i + 1] == ')') &&
                command[i] != ')'
            ) {
                sb.append(command[i])
            }
            i++
        }
        return sb.toString()
    }
}
```