[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3403\. Find the Lexicographically Largest String From the Box I

Medium

You are given a string `word`, and an integer `numFriends`.

Alice is organizing a game for her `numFriends` friends. There are multiple rounds in the game, where in each round:

*   `word` is split into `numFriends` **non-empty** strings, such that no previous round has had the **exact** same split.
*   All the split words are put into a box.

Find the **lexicographically largest** string from the box after all the rounds are finished.

A string `a` is **lexicographically smaller** than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`.   
 If the first `min(a.length, b.length)` characters do not differ, then the shorter string is the lexicographically smaller one.

**Example 1:**

**Input:** word = "dbca", numFriends = 2

**Output:** "dbc"

**Explanation:**

All possible splits are:

*   `"d"` and `"bca"`.
*   `"db"` and `"ca"`.
*   `"dbc"` and `"a"`.

**Example 2:**

**Input:** word = "gggg", numFriends = 4

**Output:** "g"

**Explanation:**

The only possible split is: `"g"`, `"g"`, `"g"`, and `"g"`.

**Constraints:**

*   <code>1 <= word.length <= 5 * 10<sup>3</sup></code>
*   `word` consists only of lowercase English letters.
*   `1 <= numFriends <= word.length`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun answerString(word: String, numFriends: Int): String {
        if (numFriends == 1) {
            return word
        }
        val n = word.length
        val maxlen = n - numFriends + 1
        var maxchar = word[0]
        var res = ""
        for (i in 0..<n) {
            if (word[i] >= maxchar) {
                val curr = word.substring(i, min((i + maxlen), n))
                if (curr > res) {
                    res = curr
                }
                maxchar = word[i]
            }
        }
        return res
    }
}
```