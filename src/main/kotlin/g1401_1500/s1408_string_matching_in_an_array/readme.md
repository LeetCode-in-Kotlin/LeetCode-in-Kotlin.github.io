[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1408\. String Matching in an Array

Easy

Given an array of string `words`. Return all strings in `words` which is substring of another word in **any** order.

String `words[i]` is substring of `words[j]`, if can be obtained removing some characters to left and/or right side of `words[j]`.

**Example 1:**

**Input:** words = ["mass","as","hero","superhero"]

**Output:** ["as","hero"]

**Explanation:** "as" is substring of "mass" and "hero" is substring of "superhero". ["hero","as"] is also a valid answer.

**Example 2:**

**Input:** words = ["leetcode","et","code"]

**Output:** ["et","code"]

**Explanation:** "et", "code" are substring of "leetcode".

**Example 3:**

**Input:** words = ["blue","green","bu"]

**Output:** []

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 30`
*   `words[i]` contains only lowercase English letters.
*   It's **guaranteed** that `words[i]` will be unique.

## Solution

```kotlin
class Solution {
    fun stringMatching(words: Array<String>): List<String> {
        val set: MutableSet<String> = HashSet()
        for (word in words) {
            for (s in words) {
                if (word != s && word.length < s.length && s.contains(word)) {
                    set.add(word)
                }
            }
        }
        return ArrayList(set)
    }
}
```