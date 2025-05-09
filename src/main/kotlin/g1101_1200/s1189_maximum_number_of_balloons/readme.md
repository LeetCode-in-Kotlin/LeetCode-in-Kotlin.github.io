[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1189\. Maximum Number of Balloons

Easy

Given a string `text`, you want to use the characters of `text` to form as many instances of the word **"balloon"** as possible.

You can use each character in `text` **at most once**. Return the maximum number of instances that can be formed.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/05/1536_ex1_upd.JPG)**

**Input:** text = "nlaebolko"

**Output:** 1

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/09/05/1536_ex2_upd.JPG)**

**Input:** text = "loonbalxballpoon"

**Output:** 2

**Example 3:**

**Input:** text = "leetcode"

**Output:** 0

**Constraints:**

*   <code>1 <= text.length <= 10<sup>4</sup></code>
*   `text` consists of lower case English letters only.

## Solution

```kotlin
class Solution {
    fun maxNumberOfBalloons(text: String): Int {
        val counts = IntArray(26)
        for (c in text.toCharArray()) {
            counts[c.code - 'a'.code]++
        }
        return Math.min(
            counts[0],
            Math.min(
                counts[1],
                Math.min(counts[11] / 2, Math.min(counts[14] / 2, counts[13])),
            ),
        )
    }
}
```