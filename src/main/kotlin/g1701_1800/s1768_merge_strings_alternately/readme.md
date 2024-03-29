[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1768\. Merge Strings Alternately

Easy

You are given two strings `word1` and `word2`. Merge the strings by adding letters in alternating order, starting with `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return _the merged string._

**Example 1:**

**Input:** word1 = "abc", word2 = "pqr"

**Output:** "apbqcr"

**Explanation:** The merged string will be merged as so:

word1:  a   b   c

word2:    p   q   r

merged: a p b q c r

**Example 2:**

**Input:** word1 = "ab", word2 = "pqrs"

**Output:** "apbqrs"

**Explanation:** Notice that as word2 is longer, "rs" is appended to the end.

word1:  a   b 

word2:    p   q   r   s

merged: a p b q   r   s

**Example 3:**

**Input:** word1 = "abcd", word2 = "pq"

**Output:** "apbqcd"

**Explanation:** Notice that as word1 is longer, "cd" is appended to the end.

word1:  a   b   c   d

word2:    p   q 

merged: a p b q c   d

**Constraints:**

*   `1 <= word1.length, word2.length <= 100`
*   `word1` and `word2` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun mergeAlternately(word1: String, word2: String): String {
        val size1 = word1.length
        val size2 = word2.length
        val min = Math.min(size1, size2)
        val sb = StringBuilder()
        for (i in 0 until min) {
            sb.append(word1[i])
            sb.append(word2[i])
        }
        if (min == size1) {
            sb.append(word2, size1, size2)
        }
        if (min == size2) {
            sb.append(word1, size2, size1)
        }
        return sb.toString()
    }
}
```