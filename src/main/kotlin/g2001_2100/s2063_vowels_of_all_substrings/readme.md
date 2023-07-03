[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2063\. Vowels of All Substrings

Medium

Given a string `word`, return _the **sum of the number of vowels** (_`'a'`, `'e'`_,_ `'i'`_,_ `'o'`_, and_ `'u'`_)_ _in every substring of_ `word`.

A **substring** is a contiguous (non-empty) sequence of characters within a string.

**Note:** Due to the large constraints, the answer may not fit in a signed 32-bit integer. Please be careful during the calculations.

**Example 1:**

**Input:** word = "aba"

**Output:** 6

**Explanation:** All possible substrings are: "a", "ab", "aba", "b", "ba", and "a". 

- "b" has 0 vowels in it 

- "a", "ab", "ba", and "a" have 1 vowel each 

- "aba" has 2 vowels in it 
  
Hence, the total sum of vowels = 0 + 1 + 1 + 1 + 1 + 2 = 6.

**Example 2:**

**Input:** word = "abc"

**Output:** 3

**Explanation:** All possible substrings are: "a", "ab", "abc", "b", "bc", and "c". 

- "a", "ab", and "abc" have 1 vowel each 

- "b", "bc", and "c" have 0 vowels each 
  
Hence, the total sum of vowels = 1 + 1 + 1 + 0 + 0 + 0 = 3.

**Example 3:**

**Input:** word = "ltcd"

**Output:** 0

**Explanation:** There are no vowels in any substring of "ltcd".

**Constraints:**

*   <code>1 <= word.length <= 10<sup>5</sup></code>
*   `word` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun countVowels(word: String): Long {
        var ans: Long = 0
        for (i in word.indices) {
            if (isVowel(word[i])) {
                val right = word.length - i.toLong() - 1
                ans += (i.toLong() + 1) * (right + 1)
            }
        }
        return ans
    }

    private fun isVowel(ch: Char): Boolean {
        return ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u'
    }
}
```