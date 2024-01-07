[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2942\. Find Words Containing Character

Easy

You are given a **0-indexed** array of strings `words` and a character `x`.

Return _an **array of indices** representing the words that contain the character_ `x`.

**Note** that the returned array may be in **any** order.

**Example 1:**

**Input:** words = ["leet","code"], x = "e"

**Output:** [0,1]

**Explanation:** "e" occurs in both words: "l**<ins>ee</ins>**t", and "cod<ins>**e**</ins>". Hence, we return indices 0 and 1. 

**Example 2:**

**Input:** words = ["abc","bcd","aaaa","cbc"], x = "a"

**Output:** [0,2]

**Explanation:** "a" occurs in "**<ins>a</ins>**bc", and "<ins>**aaaa**</ins>". Hence, we return indices 0 and 2. 

**Example 3:**

**Input:** words = ["abc","bcd","aaaa","cbc"], x = "z"

**Output:** []

**Explanation:** "z" does not occur in any of the words. Hence, we return an empty array. 

**Constraints:**

*   `1 <= words.length <= 50`
*   `1 <= words[i].length <= 50`
*   `x` is a lowercase English letter.
*   `words[i]` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun findWordsContaining(words: Array<String>, x: Char): List<Int> {
        val ans: MutableList<Int> = ArrayList()
        for (i in words.indices) {
            for (j in 0 until words[i].length) {
                if (words[i][j] == x) {
                    ans.add(i)
                    break
                }
            }
        }
        return ans
    }
}
```