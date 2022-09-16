[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 438\. Find All Anagrams in a String

Medium

Given two strings `s` and `p`, return _an array of all the start indices of_ `p`_'s anagrams in_ `s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** s = "cbaebabacd", p = "abc"

**Output:** [0,6]

**Explanation:** 

The substring with start index = 0 is "cba", which is an anagram of "abc". 

The substring with start index = 6 is "bac", which is an anagram of "abc".

**Example 2:**

**Input:** s = "abab", p = "ab"

**Output:** [0,1,2]

**Explanation:** 

The substring with start index = 0 is "ab", which is an anagram of "ab". 

The substring with start index = 1 is "ba", which is an anagram of "ab". 

The substring with start index = 2 is "ab", which is an anagram of "ab".

**Constraints:**

*   <code>1 <= s.length, p.length <= 3 * 10<sup>4</sup></code>
*   `s` and `p` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun findAnagrams(s: String, p: String): List<Int> {
        val map = IntArray(26)
        for (i in 0 until p.length) {
            map[p[i] - 'a']++
        }
        val res: MutableList<Int> = ArrayList()
        var i = 0
        var j = 0
        while (i < s.length) {
            val idx = s[i] - 'a'
            // add the new character
            map[idx]--
            // if the length is greater than windows length, pop the left charcater in the window
            if (i >= p.length) {
                map[s[j++] - 'a']++
            }
            var finish = true
            for (k in 0..25) {
                // if it is not an anagram of string p
                if (map[k] != 0) {
                    finish = false
                    break
                }
            }
            if (i >= p.length - 1 && finish) {
                res.add(j)
            }
            i++
        }
        return res
    }
}
```