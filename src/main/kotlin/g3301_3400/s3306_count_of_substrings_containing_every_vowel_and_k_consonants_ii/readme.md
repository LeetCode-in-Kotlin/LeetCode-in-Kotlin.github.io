[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3306\. Count of Substrings Containing Every Vowel and K Consonants II

Medium

You are given a string `word` and a **non-negative** integer `k`.

Return the total number of substrings of `word` that contain every vowel (`'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) **at least** once and **exactly** `k` consonants.

**Example 1:**

**Input:** word = "aeioqq", k = 1

**Output:** 0

**Explanation:**

There is no substring with every vowel.

**Example 2:**

**Input:** word = "aeiou", k = 0

**Output:** 1

**Explanation:**

The only substring with every vowel and zero consonants is `word[0..4]`, which is `"aeiou"`.

**Example 3:**

**Input:** word = "ieaouqqieaouqq", k = 1

**Output:** 3

**Explanation:**

The substrings with every vowel and one consonant are:

*   `word[0..5]`, which is `"ieaouq"`.
*   `word[6..11]`, which is `"qieaou"`.
*   `word[7..12]`, which is `"ieaouq"`.

**Constraints:**

*   <code>5 <= word.length <= 2 * 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.
*   `0 <= k <= word.length - 5`

## Solution

```kotlin
import java.util.HashMap
import java.util.HashSet

class Solution {
    fun countOfSubstrings(word: String, k: Int): Long {
        return (
            countOfSubstringHavingAtleastXConsonants(word, k) -
                countOfSubstringHavingAtleastXConsonants(word, k + 1)
            )
    }

    private fun countOfSubstringHavingAtleastXConsonants(word: String, k: Int): Long {
        var start = 0
        var end = 0
        val vowels: MutableSet<Char> = HashSet<Char>()
        vowels.add('a')
        vowels.add('e')
        vowels.add('i')
        vowels.add('o')
        vowels.add('u')
        var consonants = 0
        val map: MutableMap<Char, Int> = HashMap<Char, Int>()
        var res: Long = 0
        while (end < word.length) {
            val ch = word[end]
            // adding vowel or consonants;
            if (vowels.contains(ch)) {
                if (map.containsKey(ch)) {
                    map.put(ch, map[ch]!! + 1)
                } else {
                    map.put(ch, 1)
                }
            } else {
                consonants++
            }
            // checking any valid string ispresent or not
            while (map.size == 5 && consonants >= k) {
                res += (word.length - end).toLong()
                val ch1 = word[start]
                if (vowels.contains(ch1)) {
                    val temp = map[ch1]!! - 1
                    if (temp == 0) {
                        map.remove(ch1)
                    } else {
                        map.put(ch1, temp)
                    }
                } else {
                    consonants--
                }
                start++
            }
            end++
        }
        return res
    }
}
```