[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 884\. Uncommon Words from Two Sentences

Easy

A **sentence** is a string of single-space separated words where each word consists only of lowercase letters.

A word is **uncommon** if it appears exactly once in one of the sentences, and **does not appear** in the other sentence.

Given two **sentences** `s1` and `s2`, return _a list of all the **uncommon words**_. You may return the answer in **any order**.

**Example 1:**

**Input:** s1 = "this apple is sweet", s2 = "this apple is sour"

**Output:** ["sweet","sour"]

**Example 2:**

**Input:** s1 = "apple apple", s2 = "banana"

**Output:** ["banana"]

**Constraints:**

*   `1 <= s1.length, s2.length <= 200`
*   `s1` and `s2` consist of lowercase English letters and spaces.
*   `s1` and `s2` do not have leading or trailing spaces.
*   All the words in `s1` and `s2` are separated by a single space.

## Solution

```kotlin
class Solution {
    fun uncommonFromSentences(s1: String, s2: String): Array<String?> {
        val visited = HashSet<String>()
        val uniques = HashSet<String>()
        for (word in s1.split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()) {
            if (!visited.add(word)) {
                uniques.remove(word)
            } else {
                uniques.add(word)
            }
        }
        for (word in s2.split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()) {
            if (!visited.add(word)) {
                uniques.remove(word)
            } else {
                uniques.add(word)
            }
        }
        val arr = arrayOfNulls<String>(uniques.size)
        for ((i, word) in uniques.withIndex()) {
            arr[i] = word
        }
        return arr
    }
}
```