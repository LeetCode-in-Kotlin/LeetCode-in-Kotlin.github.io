[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1957\. Delete Characters to Make Fancy String

Easy

A **fancy string** is a string where no **three** **consecutive** characters are equal.

Given a string `s`, delete the **minimum** possible number of characters from `s` to make it **fancy**.

Return _the final string after the deletion_. It can be shown that the answer will always be **unique**.

**Example 1:**

**Input:** s = "leeetcode"

**Output:** "leetcode"

**Explanation:** 

Remove an 'e' from the first group of 'e's to create "leetcode". 

No three consecutive characters are equal, so return "leetcode".

**Example 2:**

**Input:** s = "aaabaaaa"

**Output:** "aabaa"

**Explanation:** 

Remove an 'a' from the first group of 'a's to create "aabaaaa". 

Remove two 'a's from the second group of 'a's to create "aabaa".

No three consecutive characters are equal, so return "aabaa".

**Example 3:**

**Input:** s = "aab"

**Output:** "aab"

**Explanation:** No three consecutive characters are equal, so return "aab".

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun makeFancyString(s: String): String {
        val ans = StringBuilder()
        var c = 1
        ans.append(s[0])
        for (i in 1 until s.length) {
            if (s[i] == s[i - 1]) {
                c++
            } else {
                c = 1
            }
            if (c < 3) {
                ans.append(s[i])
            }
        }
        return ans.toString()
    }
}
```