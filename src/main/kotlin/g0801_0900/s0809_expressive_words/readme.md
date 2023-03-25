[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 809\. Expressive Words

Medium

Sometimes people repeat letters to represent extra feeling. For example:

*   `"hello" -> "heeellooo"`
*   `"hi" -> "hiiii"`

In these strings like `"heeellooo"`, we have groups of adjacent letters that are all the same: `"h"`, `"eee"`, `"ll"`, `"ooo"`.

You are given a string `s` and an array of query strings `words`. A query word is **stretchy** if it can be made to be equal to `s` by any number of applications of the following extension operation: choose a group consisting of characters `c`, and add some number of characters `c` to the group so that the size of the group is **three or more**.

*   For example, starting with `"hello"`, we could do an extension on the group `"o"` to get `"hellooo"`, but we cannot get `"helloo"` since the group `"oo"` has a size less than three. Also, we could do another extension like `"ll" -> "lllll"` to get `"helllllooo"`. If `s = "helllllooo"`, then the query word `"hello"` would be **stretchy** because of these two extension operations: `query = "hello" -> "hellooo" -> "helllllooo" = s`.

Return _the number of query strings that are **stretchy**_.

**Example 1:**

**Input:** s = "heeellooo", words = ["hello", "hi", "helo"]

**Output:** 1

**Explanation:** 

We can extend "e" and "o" in the word "hello" to get "heeellooo". 

We can't extend "helo" to get "heeellooo" because the group "ll" is not size 3 or more.

**Example 2:**

**Input:** s = "zzzzzyyyyy", words = ["zzyy","zy","zyy"]

**Output:** 3

**Constraints:**

*   `1 <= s.length, words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `s` and `words[i]` consist of lowercase letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun expressiveWords(s: String, words: Array<String>): Int {
        var ans = 0
        for (w in words) {
            if (check(s, w)) {
                ans++
            }
        }
        return ans
    }

    private fun check(s: String, w: String): Boolean {
        var i = 0
        var j = 0
        while (i < s.length && j < w.length) {
            val ch1 = s[i]
            val ch2 = w[j]
            val len1 = getLen(s, i)
            val len2 = getLen(w, j)
            if (ch1 == ch2) {
                if (len1 == len2 || len1 >= 3 && len2 < len1) {
                    i += len1
                    j += len2
                } else {
                    return false
                }
            } else {
                return false
            }
        }
        return i == s.length && j == w.length
    }

    private fun getLen(value: String, i: Int): Int {
        var i = i
        i += 1
        var count = 1
        for (j in i until value.length) {
            if (value[j] == value[i - 1]) {
                count++
            } else {
                break
            }
        }
        return count
    }
}
```