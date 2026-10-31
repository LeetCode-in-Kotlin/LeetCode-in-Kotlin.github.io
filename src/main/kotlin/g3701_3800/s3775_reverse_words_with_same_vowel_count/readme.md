[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3775\. Reverse Words With Same Vowel Count

Medium

You are given a string `s` consisting of lowercase English words, each separated by a single space.

Determine how many vowels appear in the **first** word. Then, reverse each following word that has the **same vowel count**. Leave all remaining words unchanged.

Return the resulting string.

Vowels are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**

**Input:** s = "cat and mice"

**Output:** "cat dna mice"

**Explanation:**

*   The first word `"cat"` has 1 vowel.
*   `"and"` has 1 vowel, so it is reversed to form `"dna"`.
*   `"mice"` has 2 vowels, so it remains unchanged.
*   Thus, the resulting string is `"cat dna mice"`.

**Example 2:**

**Input:** s = "book is nice"

**Output:** "book is ecin"

**Explanation:**

*   The first word `"book"` has 2 vowels.
*   `"is"` has 1 vowel, so it remains unchanged.
*   `"nice"` has 2 vowels, so it is reversed to form `"ecin"`.
*   Thus, the resulting string is `"book is ecin"`.

**Example 3:**

**Input:** s = "banana healthy"

**Output:** "banana healthy"

**Explanation:**

*   The first word `"banana"` has 3 vowels.
*   `"healthy"` has 2 vowels, so it remains unchanged.
*   Thus, the resulting string is `"banana healthy"`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters and spaces.
*   Words in `s` are separated by a **single** space.
*   `s` does **not** contain leading or trailing spaces.

## Solution

```kotlin
class Solution {
    fun reverseWords(s: String): String {
        val wrd = s.toCharArray()
        val vowels = "aeiou"
        var left = 0
        var right: Int
        var firstWrd = 0
        var anotherWrd: Int
        while (left < wrd.size) {
            right = left
            anotherWrd = 0
            while (right < wrd.size && wrd[right] != ' ') {
                if (vowels.indexOf(wrd[right]) != -1) {
                    if (left == 0) {
                        firstWrd++
                    } else {
                        anotherWrd++
                    }
                }
                right++
            }
            if (left != 0 && anotherWrd == firstWrd) {
                var l = left
                var r = right - 1
                while (l < r) {
                    val temp = wrd[r]
                    wrd[r--] = wrd[l]
                    wrd[l++] = temp
                }
            }
            left = right + 1
        }
        return String(wrd)
    }
}
```