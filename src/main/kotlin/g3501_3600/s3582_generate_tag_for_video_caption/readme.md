[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3582\. Generate Tag for Video Caption

Easy

You are given a string `caption` representing the caption for a video.

The following actions must be performed **in order** to generate a **valid tag** for the video:

1.  **Combine all words** in the string into a single _camelCase string_ prefixed with `'#'`. A _camelCase string_ is one where the first letter of all words _except_ the first one is capitalized. All characters after the first character in **each** word must be lowercase.
    
2.  **Remove** all characters that are not an English letter, **except** the first `'#'`.
    
3.  **Truncate** the result to a maximum of 100 characters.
    

Return the **tag** after performing the actions on `caption`.

**Example 1:**

**Input:** caption = "Leetcode daily streak achieved"

**Output:** "#leetcodeDailyStreakAchieved"

**Explanation:**

The first letter for all words except `"leetcode"` should be capitalized.

**Example 2:**

**Input:** caption = "can I Go There"

**Output:** "#canIGoThere"

**Explanation:**

The first letter for all words except `"can"` should be capitalized.

**Example 3:**

**Input:** caption = "hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh"

**Output:** "#hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh"

**Explanation:**

Since the first word has length 101, we need to truncate the last two letters from the word.

**Constraints:**

*   `1 <= caption.length <= 150`
*   `caption` consists only of English letters and `' '`.

## Solution

```kotlin
class Solution {
    fun generateTag(caption: String): String? {
        var caption = caption
        val sb = StringBuilder()
        sb.append('#')
        var space = false
        caption = caption.trim { it <= ' ' }
        for (i in 0..<caption.length) {
            val c = caption[i]
            if (c == ' ') {
                space = true
            }
            if (c >= 'A' && c <= 'Z') {
                if (space) {
                    space = !space
                    sb.append(c)
                } else {
                    sb.append(c.lowercaseChar())
                }
            }
            if (c >= 'a' && c <= 'z') {
                if (space) {
                    space = !space
                    sb.append(c.uppercaseChar())
                } else {
                    sb.append(c)
                }
            }
        }
        if (sb.length > 100) {
            return sb.substring(0, 100)
        }
        return sb.toString()
    }
}
```