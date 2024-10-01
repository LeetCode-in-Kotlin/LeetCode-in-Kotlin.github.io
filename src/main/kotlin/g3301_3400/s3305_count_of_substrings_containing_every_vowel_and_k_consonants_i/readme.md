[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3305\. Count of Substrings Containing Every Vowel and K Consonants I

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

*   `5 <= word.length <= 250`
*   `word` consists only of lowercase English letters.
*   `0 <= k <= word.length - 5`

## Solution

```kotlin
class Solution {
    fun countOfSubstrings(word: String, k: Int): Int {
        val arr = word.toCharArray()
        val map = IntArray(26)
        map[0]++
        map['e'.code - 'a'.code]++
        map['i'.code - 'a'.code]++
        map['o'.code - 'a'.code]++
        map['u'.code - 'a'.code]++
        var need = 5
        var ans = 0
        var consCnt = 0
        var j = 0
        for (i in arr.indices) {
            while (j < arr.size && (need > 0 || consCnt < k)) {
                if (isVowel(arr[j])) {
                    map[arr[j].code - 'a'.code]--
                    if (map[arr[j].code - 'a'.code] == 0) {
                        need--
                    }
                } else {
                    consCnt++
                }
                j++
            }
            if (need == 0 && consCnt == k) {
                ans++
                var m = j
                while (m < arr.size) {
                    if (isVowel(arr[m])) {
                        ans++
                    } else {
                        break
                    }
                    m++
                }
            }
            if (isVowel(arr[i])) {
                map[arr[i].code - 'a'.code]++
                if (map[arr[i].code - 'a'.code] == 1) {
                    need++
                }
            } else {
                consCnt--
            }
        }
        return ans
    }

    private fun isVowel(ch: Char): Boolean {
        return ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u'
    }
}
```