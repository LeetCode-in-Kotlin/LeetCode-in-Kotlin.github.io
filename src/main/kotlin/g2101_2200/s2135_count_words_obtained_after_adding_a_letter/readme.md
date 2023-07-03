[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2135\. Count Words Obtained After Adding a Letter

Medium

You are given two **0-indexed** arrays of strings `startWords` and `targetWords`. Each string consists of **lowercase English letters** only.

For each string in `targetWords`, check if it is possible to choose a string from `startWords` and perform a **conversion operation** on it to be equal to that from `targetWords`.

The **conversion operation** is described in the following two steps:

1.  **Append** any lowercase letter that is **not present** in the string to its end.
    *   For example, if the string is `"abc"`, the letters `'d'`, `'e'`, or `'y'` can be added to it, but not `'a'`. If `'d'` is added, the resulting string will be `"abcd"`.
2.  **Rearrange** the letters of the new string in **any** arbitrary order.
    *   For example, `"abcd"` can be rearranged to `"acbd"`, `"bacd"`, `"cbda"`, and so on. Note that it can also be rearranged to `"abcd"` itself.

Return _the **number of strings** in_ `targetWords` _that can be obtained by performing the operations on **any** string of_ `startWords`.

**Note** that you will only be verifying if the string in `targetWords` can be obtained from a string in `startWords` by performing the operations. The strings in `startWords` **do not** actually change during this process.

**Example 1:**

**Input:** startWords = ["ant","act","tack"], targetWords = ["tack","act","acti"]

**Output:** 2

**Explanation:** 

- In order to form targetWords[0] = "tack", we use startWords[1] = "act", append 'k' to it, and rearrange "actk" to "tack". 

- There is no string in startWords that can be used to obtain targetWords[1] = "act". 
  
Note that "act" does exist in startWords, but we **must** append one letter to the string before rearranging it. 

- In order to form targetWords[2] = "acti", we use startWords[1] = "act", append 'i' to it, and rearrange "acti" to "acti" itself.

**Example 2:**

**Input:** startWords = ["ab","a"], targetWords = ["abc","abcd"]

**Output:** 1

**Explanation:** 

- In order to form targetWords[0] = "abc", we use startWords[0] = "ab", add 'c' to it, and rearrange it to "abc". 

- There is no string in startWords that can be used to obtain targetWords[1] = "abcd".

**Constraints:**

*   <code>1 <= startWords.length, targetWords.length <= 5 * 10<sup>4</sup></code>
*   `1 <= startWords[i].length, targetWords[j].length <= 26`
*   Each string of `startWords` and `targetWords` consists of lowercase English letters only.
*   No letter occurs more than once in any string of `startWords` or `targetWords`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private lateinit var set: MutableSet<Int>

    private fun preprocess(words: Array<String>) {
        set = HashSet()
        for (word in words) {
            val bitMap = getBitMap(word)
            set.add(bitMap)
        }
    }

    private fun matches(bitMap: Int): Boolean {
        return set.contains(bitMap)
    }

    private fun getBitMap(word: String): Int {
        var result = 0
        for (element in word) {
            val position = element.code - 'a'.code
            result = result or (1 shl position)
        }
        return result
    }

    private fun addBit(bitMap: Int, c: Char): Int {
        var bitMap = bitMap
        val position = c.code - 'a'.code
        bitMap = bitMap or (1 shl position)
        return bitMap
    }

    private fun removeBit(bitMap: Int, c: Char): Int {
        var bitMap = bitMap
        val position = c.code - 'a'.code
        bitMap = bitMap and (1 shl position).inv()
        return bitMap
    }

    fun wordCount(startWords: Array<String>, targetWords: Array<String>): Int {
        if (startWords.isEmpty()) {
            return 0
        }
        if (targetWords.isEmpty()) {
            return 0
        }
        preprocess(startWords)
        var count = 0
        for (word in targetWords) {
            var bitMap = getBitMap(word)
            for (i in word.indices) {
                bitMap = removeBit(bitMap, word[i])
                if (i > 0) {
                    bitMap = addBit(bitMap, word[i - 1])
                }
                if (matches(bitMap)) {
                    count++
                    break
                }
            }
        }
        return count
    }
}
```