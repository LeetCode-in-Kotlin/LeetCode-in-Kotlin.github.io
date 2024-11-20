[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 937\. Reorder Data in Log Files

Medium

You are given an array of `logs`. Each log is a space-delimited string of words, where the first word is the **identifier**.

There are two types of logs:

*   **Letter-logs**: All words (except the identifier) consist of lowercase English letters.
*   **Digit-logs**: All words (except the identifier) consist of digits.

Reorder these logs so that:

1.  The **letter-logs** come before all **digit-logs**.
2.  The **letter-logs** are sorted lexicographically by their contents. If their contents are the same, then sort them lexicographically by their identifiers.
3.  The **digit-logs** maintain their relative ordering.

Return _the final order of the logs_.

**Example 1:**

**Input:** logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]

**Output:** ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]

**Explanation:** 

The letter-log contents are all different, so their ordering is "art can", "art zero", "own kit dig". 

The digit-logs have a relative order of "dig1 8 1 5 1", "dig2 3 6".

**Example 2:**

**Input:** logs = ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]

**Output:** ["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]

**Constraints:**

*   `1 <= logs.length <= 100`
*   `3 <= logs[i].length <= 100`
*   All the tokens of `logs[i]` are separated by a **single** space.
*   `logs[i]` is guaranteed to have an identifier and at least one word after the identifier.

## Solution

```kotlin
import java.util.Collections

class Solution {
    fun reorderLogFiles(logs: Array<String>): Array<String?> {
        val digi: MutableList<String> = ArrayList()
        val word: MutableList<String> = ArrayList()
        for (s in logs) {
            if (Character.isDigit(s[s.length - 1])) digi.add(s) else word.add(s)
        }
        Collections.sort(
            word,
            Comparator { s1, s2 ->
                val firstSpacePosition = s1.indexOf(" ")
                val firstWord = s1.substring(firstSpacePosition, s1.length)
                val secondSpacePosition = s2.indexOf(" ")
                val secondWord = s2.substring(secondSpacePosition, s2.length)
                if (firstWord.compareTo(secondWord) == 0) {
                    val firstSpacePosition1 = s1.indexOf(" ")
                    val firstWord1 = s1.substring(0, firstSpacePosition1)
                    val secondSpacePosition1 = s2.indexOf(" ")
                    val secondWord1 = s2.substring(0, secondSpacePosition1)
                    return@Comparator firstWord1.compareTo(secondWord1)
                }
                firstWord.compareTo(secondWord)
            },
        )
        val result = arrayOfNulls<String>(digi.size + word.size)
        var `in` = 0
        for (s in word) result[`in`++] = s
        for (s in digi) result[`in`++] = s
        return result
    }
}
```