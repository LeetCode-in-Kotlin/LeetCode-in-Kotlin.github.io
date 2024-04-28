[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3120\. Count the Number of Special Characters I

Easy

You are given a string `word`. A letter is called **special** if it appears **both** in lowercase and uppercase in `word`.

Return the number of **special** letters in `word`.

**Example 1:**

**Input:** word = "aaAbcBC"

**Output:** 3

**Explanation:**

The special characters in `word` are `'a'`, `'b'`, and `'c'`.

**Example 2:**

**Input:** word = "abc"

**Output:** 0

**Explanation:**

No character in `word` appears in uppercase.

**Example 3:**

**Input:** word = "abBCab"

**Output:** 1

**Explanation:**

The only special character in `word` is `'b'`.

**Constraints:**

*   `1 <= word.length <= 50`
*   `word` consists of only lowercase and uppercase English letters.

## Solution

```kotlin
class Solution {
    fun numberOfSpecialChars(word: String): Int {
        val a = IntArray(26)
        val b = IntArray(26)
        var ans = 0
        for (c in word.toCharArray()) {
            if (c in 'a'..'z') {
                a[c.code - 'a'.code]++
            } else {
                b[c.code - 'A'.code]++
            }
        }
        for (i in 0..25) {
            if (a[i] != 0 && b[i] != 0) {
                ans++
            }
        }
        return ans
    }
}
```