[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1544\. Make The String Great

Easy

Given a string `s` of lower and upper case English letters.

A good string is a string which doesn't have **two adjacent characters** `s[i]` and `s[i + 1]` where:

*   `0 <= i <= s.length - 2`
*   `s[i]` is a lower-case letter and `s[i + 1]` is the same letter but in upper-case or **vice-versa**.

To make the string good, you can choose **two adjacent** characters that make the string bad and remove them. You can keep doing this until the string becomes good.

Return _the string_ after making it good. The answer is guaranteed to be unique under the given constraints.

**Notice** that an empty string is also good.

**Example 1:**

**Input:** s = "leEeetcode"

**Output:** "leetcode"

**Explanation:** In the first step, either you choose i = 1 or i = 2, both will result "leEeetcode" to be reduced to "leetcode".

**Example 2:**

**Input:** s = "abBAcC"

**Output:** ""

**Explanation:** We have many possible scenarios, and all lead to the same answer. For example: "abBAcC" --> "aAcC" --> "cC" --> "" "abBAcC" --> "abBA" --> "aA" --> ""

**Example 3:**

**Input:** s = "s"

**Output:** "s"

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` contains only lower and upper case English letters.

## Solution

```kotlin
class Solution {
    fun makeGood(s: String): String {
        val stack = ArrayDeque<Char>()
        for (element in s) {
            if (stack.isEmpty()) {
                stack.add(element)
            } else {
                if (stack.last().lowercaseChar() == element.lowercaseChar()) {
                    if (Character.isLowerCase(stack.last()) && Character.isUpperCase(element)) {
                        stack.removeLast()
                    } else if (Character.isUpperCase(stack.last()) && Character.isLowerCase(element)) {
                        stack.removeLast()
                    } else {
                        stack.add(element)
                    }
                } else {
                    stack.add(element)
                }
            }
        }
        val sb = StringBuilder()
        while (stack.isNotEmpty()) {
            sb.append(stack.removeLast())
        }
        return sb.reverse().toString()
    }
}
```