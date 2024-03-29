[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2423\. Remove Letter To Equalize Frequency

Easy

You are given a **0-indexed** string `word`, consisting of lowercase English letters. You need to select **one** index and **remove** the letter at that index from `word` so that the **frequency** of every letter present in `word` is equal.

Return `true` _if it is possible to remove one letter so that the frequency of all letters in_ `word` _are equal, and_ `false` _otherwise_.

**Note:**

*   The **frequency** of a letter `x` is the number of times it occurs in the string.
*   You **must** remove exactly one letter and cannot chose to do nothing.

**Example 1:**

**Input:** word = "abcc"

**Output:** true

**Explanation:** Select index 3 and delete it: word becomes "abc" and each character has a frequency of 1. 

**Example 2:**

**Input:** word = "aazz"

**Output:** false

**Explanation:** We must delete a character, so either the frequency of "a" is 1 and the frequency of "z" is 2, or vice versa. It is impossible to make all present letters have equal frequency. 

**Constraints:**

*   `2 <= word.length <= 100`
*   `word` consists of lowercase English letters only.

## Solution

```kotlin
class Solution {
    private fun isValidWord(arr: IntArray): Boolean {
        var temp = 0
        for (j in 0..25) {
            if (arr[j] == 0) {
                continue
            }
            if (temp == 0) {
                temp = arr[j]
                continue
            }
            if (arr[j] != temp) {
                return false
            }
        }
        return true
    }

    fun equalFrequency(word: String): Boolean {
        var ans = false
        // frequency array
        val arr = IntArray(26)
        for (element in word) {
            arr[element.code - 'a'.code] += 1
        }
        for (i in 0..25) {
            if (arr[i] != 0) {
                arr[i] -= 1
                if (isValidWord(arr)) {
                    ans = true
                    break
                }
                arr[i] += 1
            }
        }
        return ans
    }
}
```