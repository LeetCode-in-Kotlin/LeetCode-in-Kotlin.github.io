[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1790\. Check if One String Swap Can Make Strings Equal

Easy

You are given two strings `s1` and `s2` of equal length. A **string swap** is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.

Return `true` _if it is possible to make both strings equal by performing **at most one string swap** on **exactly one** of the strings._ Otherwise, return `false`.

**Example 1:**

**Input:** s1 = "bank", s2 = "kanb"

**Output:** true

**Explanation:** For example, swap the first character with the last character of s2 to make "bank". 

**Example 2:**

**Input:** s1 = "attack", s2 = "defend"

**Output:** false

**Explanation:** It is impossible to make them equal with one string swap. 

**Example 3:**

**Input:** s1 = "kelb", s2 = "kelb"

**Output:** true

**Explanation:** The two strings are already equal, so no string swap operation is required. 

**Constraints:**

*   `1 <= s1.length, s2.length <= 100`
*   `s1.length == s2.length`
*   `s1` and `s2` consist of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun areAlmostEqual(s1: String, s2: String): Boolean {
        var i1 = -1
        var i2 = -1
        // We go though the two strings
        for (i in 0 until s1.length) {
            // check if each char is the same.
            if (s1[i] == s2[i]) {
                continue
            }
            // When there are more than 2 char different., we return false;
            if (i2 != -1) {
                return false
            }
            // If there is char that is different, we record the index.
            if (i1 == -1) {
                i1 = i
            } else {
                // If there is char that is different, we record the index.
                i2 = i
            }
        }
        // When three is no different char, we return true;
        if (i1 == i2) {
            return true
        }
        // When there is 1 char different, we return false;
        return if (i2 == -1) {
            false
        } else {
            s1[i1] == s2[i2] && s1[i2] == s2[i1]
        }
        // When there are 2 char different, and swap them can make two string equal, we return true;
    }
}
```