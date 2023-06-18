[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1717\. Maximum Score From Removing Substrings

Medium

You are given a string `s` and two integers `x` and `y`. You can perform two types of operations any number of times.

*   Remove substring `"ab"` and gain `x` points.
    *   For example, when removing `"ab"` from `"cabxbae"` it becomes `"cxbae"`.
*   Remove substring `"ba"` and gain `y` points.
    *   For example, when removing `"ba"` from `"cabxbae"` it becomes `"cabxe"`.

Return _the maximum points you can gain after applying the above operations on_ `s`.

**Example 1:**

**Input:** s = "cdbcbbaaabab", x = 4, y = 5

**Output:** 19

**Explanation:** 

- Remove the "ba" underlined in "cdbcbbaaabab". Now, s = "cdbcbbaaab" and 5 points are added to the score. 

- Remove the "ab" underlined in "cdbcbbaaab". Now, s = "cdbcbbaa" and 4 points are added to the score. 

- Remove the "ba" underlined in "cdbcbbaa". Now, s = "cdbcba" and 5 points are added to the score. - Remove the "ba" underlined in "cdbcba". Now, s = "cdbc" and 5 points are added to the score. Total score = 5 + 4 + 5 + 5 = 19.

**Example 2:**

**Input:** s = "aabbaaxybbaabb", x = 5, y = 4

**Output:** 20

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   <code>1 <= x, y <= 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun maximumGain(s: String, x: Int, y: Int): Int {
        val v = s.toCharArray()
        return if (x > y) {
            helper(v, 'a', 'b', x) + helper(v, 'b', 'a', y)
        } else {
            helper(v, 'b', 'a', y) + helper(v, 'a', 'b', x)
        }
    }

    private fun helper(v: CharArray, c1: Char, c2: Char, score: Int): Int {
        var left = -1
        var right = 0
        var res = 0
        while (right < v.size) {
            if (v[right] != c2) {
                left = right
            } else {
                while (left >= 0) {
                    val cl = v[left]
                    if (cl == '#') {
                        left--
                    } else if (cl == c1) {
                        res += score
                        v[left] = '#'
                        v[right] = '#'
                        left--
                        break
                    } else {
                        break
                    }
                }
            }
            right++
        }
        return res
    }
}
```