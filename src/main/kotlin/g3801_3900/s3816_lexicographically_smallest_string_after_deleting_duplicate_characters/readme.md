[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3816\. Lexicographically Smallest String After Deleting Duplicate Characters

Hard

You are given a string `s` that consists of lowercase English letters.

You can perform the following operation any number of times (possibly zero times):

*   Choose any letter that appears **at least twice** in the current string `s` and delete any **one** occurrence.

Return the **lexicographically smallest** resulting string that can be formed this way.

**Example 1:**

**Input:** s = "aaccb"

**Output:** "aacb"

**Explanation:**

We can form the strings `"acb"`, `"aacb"`, `"accb"`, and `"aaccb"`. `"aacb"` is the lexicographically smallest one.

For example, we can obtain `"aacb"` by choosing `'c'` and deleting its first occurrence.

**Example 2:**

**Input:** s = "z"

**Output:** "z"

**Explanation:**

We cannot perform any operations. The only string we can form is `"z"`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` contains lowercase English letters only.

## Solution

```kotlin
class Solution {
    fun lexSmallestAfterDeletion(s: String): String {
        val a = s.toCharArray()
        val rem = IntArray(26)
        for (c in a) {
            rem[c.code - 'a'.code]++
        }
        val cnt = IntArray(26)
        val sb = StringBuilder()
        for (c in a) {
            rem[c.code - 'a'.code]--
            while (sb.isNotEmpty()) {
                val t = sb[sb.length - 1]
                if (t > c && (rem[t.code - 'a'.code] > 0 || cnt[t.code - 'a'.code] > 1)) {
                    cnt[t.code - 'a'.code]--
                    sb.setLength(sb.length - 1)
                } else {
                    break
                }
            }
            sb.append(c)
            cnt[c.code - 'a'.code]++
        }
        while (sb.isNotEmpty()) {
            val i = sb.length - 1
            val c = sb[i]
            if (cnt[c.code - 'a'.code] > 1) {
                cnt[c.code - 'a'.code]--
                sb.setLength(i)
            } else {
                break
            }
        }
        return sb.toString()
    }
}
```