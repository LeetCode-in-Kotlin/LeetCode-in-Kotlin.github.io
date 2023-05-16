[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 984\. String Without AAA or BBB

Medium

Given two integers `a` and `b`, return **any** string `s` such that:

*   `s` has length `a + b` and contains exactly `a` `'a'` letters, and exactly `b` `'b'` letters,
*   The substring `'aaa'` does not occur in `s`, and
*   The substring `'bbb'` does not occur in `s`.

**Example 1:**

**Input:** a = 1, b = 2

**Output:** "abb"

**Explanation:** "abb", "bab" and "bba" are all correct answers.

**Example 2:**

**Input:** a = 4, b = 1

**Output:** "aabaa"

**Constraints:**

*   `0 <= a, b <= 100`
*   It is guaranteed such an `s` exists for the given `a` and `b`.

## Solution

```kotlin
class Solution {
    fun strWithout3a3b(a: Int, b: Int): String {
        val first = if (a > b) "a" else "b"
        val second = if (first == "a") "b" else "a"
        var firstLen = a.coerceAtLeast(b)
        var secondLen = a.coerceAtMost(b)
        val ans = StringBuilder()
        // Case 1 : A and B count are unequal.
        while (firstLen > 0 && secondLen > 0 && firstLen != secondLen) {
            ans.append(first)
            ans.append(first)
            firstLen = firstLen - 2
            ans.append(second)
            secondLen--
        }
        // Case 2: A and B count are equal
        while (firstLen > 0 && secondLen > 0) {
            ans.append(first)
            ans.append(second)
            firstLen--
            secondLen--
        }
        // left over, just append
        while (firstLen > 0) {
            ans.append(first)
            firstLen--
        }
        return ans.toString()
    }
}
```