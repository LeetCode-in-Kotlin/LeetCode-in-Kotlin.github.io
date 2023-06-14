[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1451\. Rearrange Words in a Sentence

Medium

Given a sentence `text` (A _sentence_ is a string of space-separated words) in the following format:

*   First letter is in upper case.
*   Each word in `text` are separated by a single space.

Your task is to rearrange the words in text such that all words are rearranged in an increasing order of their lengths. If two words have the same length, arrange them in their original order.

Return the new text following the format shown above.

**Example 1:**

**Input:** text = "Leetcode is cool"

**Output:** "Is cool leetcode"

**Explanation:** There are 3 words, "Leetcode" of length 8, "is" of length 2 and "cool" of length 4. Output is ordered by length and the new first word starts with capital letter.

**Example 2:**

**Input:** text = "Keep calm and code on"

**Output:** "On and keep calm code"

**Explanation:** Output is ordered as follows: 

"On" 2 letters.

"and" 3 letters. 

"keep" 4 letters in case of tie order by position in original text.

"calm" 4 letters. 

"code" 4 letters.

**Example 3:**

**Input:** text = "To be or not to be"

**Output:** "To be or to be not"

**Constraints:**

*   `text` begins with a capital letter and then contains lowercase letters and single space between words.
*   `1 <= text.length <= 10^5`

## Solution

```kotlin
import java.util.TreeMap

class Solution {
    fun arrangeWords(text: String): String {
        val map = TreeMap<Int, MutableList<String>>()
        val words = text.split(" ").dropLastWhile { it.isEmpty() }.toTypedArray()
        for (word in words) {
            val len = word.length
            map.putIfAbsent(len, ArrayList())
            map[len]!!.add(word.lowercase())
        }
        val sb = StringBuilder()
        var first = true
        for ((_, strings) in map) {
            for (str in strings) {
                val localStr = if (first) {
                    first = false
                    str[0].uppercaseChar().toString() + str.substring(1)
                } else {
                    str
                }
                sb.append(localStr).append(" ")
            }
        }
        return sb.substring(0, sb.length - 1)
    }
}
```