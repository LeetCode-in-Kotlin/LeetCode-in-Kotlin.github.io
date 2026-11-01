[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3856\. Trim Trailing Vowels

Easy

You are given a string `s` that consists of lowercase English letters.

Return the string obtained by removing **all** trailing **vowels** from `s`.

The **vowels** consist of the characters `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**

**Input:** s = "idea"

**Output:** "id"

**Explanation:**

Removing <code>"id<ins>**ea**</ins>"</code>, we obtain the string `"id"`.

**Example 2:**

**Input:** s = "day"

**Output:** "day"

**Explanation:**

There are no trailing vowels in the string `"day"`.

**Example 3:**

**Input:** s = "aeiou"

**Output:** ""

**Explanation:**

Removing <code>"<ins>**aeiou**</ins>"</code>, we obtain the string `""`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun trimTrailingVowels(s: String): String {
        var i = s.length - 1
        while (i >= 0 &&
            (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u')
        ) {
            i--
        }
        return s.substring(0, i + 1)
    }
}
```