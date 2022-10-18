[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 187\. Repeated DNA Sequences

Medium

The **DNA sequence** is composed of a series of nucleotides abbreviated as `'A'`, `'C'`, `'G'`, and `'T'`.

*   For example, `"ACGAATTCCG"` is a **DNA sequence**.

When studying **DNA**, it is useful to identify repeated sequences within the DNA.

Given a string `s` that represents a **DNA sequence**, return all the **`10`\-letter-long** sequences (substrings) that occur more than once in a DNA molecule. You may return the answer in **any order**.

**Example 1:**

**Input:** s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

**Output:** ["AAAAACCCCC","CCCCCAAAAA"]

**Example 2:**

**Input:** s = "AAAAAAAAAAAAA"

**Output:** ["AAAAAAAAAA"]

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'A'`, `'C'`, `'G'`, or `'T'`.

## Solution

```kotlin
class Solution {
    fun findRepeatedDnaSequences(s: String): List<String> {
        if (s.length < 10) {
            return emptyList()
        }
        val seen = BooleanArray(1024 * 1024)
        val added = BooleanArray(1024 * 1024)
        val chars = s.toCharArray()
        var buf = 0
        val map = IntArray(128)
        map['A'.code] = 0
        map['C'.code] = 1
        map['G'.code] = 2
        map['T'.code] = 3
        val ans: MutableList<String> = ArrayList(s.length / 2)
        for (i in 0..9) {
            buf = (buf shl 2) + map[chars[i].code]
        }
        seen[buf] = true
        for (i in 10 until chars.size) {
            buf = (buf shl 2 and 0xFFFFF) + map[chars[i].code]
            if (seen[buf]) {
                if (!added[buf]) {
                    ans.add(String(chars, i - 9, 10))
                    added[buf] = true
                }
            } else {
                seen[buf] = true
            }
        }
        return ans
    }
}
```