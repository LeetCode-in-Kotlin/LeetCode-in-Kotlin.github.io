[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 30\. Substring with Concatenation of All Words

Hard

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated substring** in `s` is a substring that contains all the strings of any permutation of `words` concatenated.

*   For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated substring because it is not the concatenation of any permutation of `words`.

Return _the starting indices of all the concatenated substrings in_ `s`. You can return the answer in **any order**.

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]

**Output:** [0,9]

**Explanation:** 

Since words.length == 2 and words[i].length == 3, the concatenated substring has to be of length 6. 

The substring starting at 0 is "barfoo". It is the concatenation of ["bar","foo"] which is a permutation of words. 

The substring starting at 9 is "foobar". It is the concatenation of ["foo","bar"] which is a permutation of words. 

The output order does not matter. Returning [9,0] is fine too.

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

**Output:** []

**Explanation:** 

Since words.length == 4 and words[i].length == 4, the concatenated substring has to be of length 16. 

There is no substring of length 16 is s that is equal to the concatenation of any permutation of words. 

We return an empty array.

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

**Output:** [6,9,12]

**Explanation:** 

Since words.length == 3 and words[i].length == 3, the concatenated substring has to be of length 9. 

The substring starting at 6 is "foobarthe". It is the concatenation of ["foo","bar","the"] which is a permutation of words.

The substring starting at 9 is "barthefoo". It is the concatenation of ["bar","the","foo"] which is a permutation of words.

The substring starting at 12 is "thefoobar". It is the concatenation of ["the","foo","bar"] which is a permutation of words.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `1 <= words.length <= 5000`
*   `1 <= words[i].length <= 30`
*   `s` and `words[i]` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun findSubstring(s: String, words: Array<String>): List<Int> {
        val indices: MutableList<Int> = ArrayList()
        if (words.size == 0) {
            return indices
        }
        // Put each word into a HashMap and calculate word frequency
        val wordMap: MutableMap<String, Int> = HashMap()
        for (word in words) {
            wordMap[word] = wordMap.getOrDefault(word, 0) + 1
        }
        val wordLength = words[0].length
        val window = words.size * wordLength
        for (i in 0 until wordLength) {
            // move a word's length each time
            var j = i
            while (j + window <= s.length) {
                // get the subStr
                val subStr = s.substring(j, j + window)
                val map: MutableMap<String, Int> = HashMap()
                // start from the last word
                for (k in words.indices.reversed()) {
                    // get the word from subStr
                    val word = subStr.substring(k * wordLength, (k + 1) * wordLength)
                    val count = map.getOrDefault(word, 0) + 1
                    // if the num of the word is greater than wordMap's, move (k * wordLength) and
                    // break
                    if (count > wordMap.getOrDefault(word, 0)) {
                        j = j + k * wordLength
                        break
                    } else if (k == 0) {
                        indices.add(j)
                    } else {
                        map[word] = count
                    }
                }
                j = j + wordLength
            }
        }
        return indices
    }
}
```