[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 14\. Longest Common Prefix

Easy

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

**Input:** strs = ["flower","flow","flight"]

**Output:** "fl" 

**Example 2:**

**Input:** strs = ["dog","racecar","car"]

**Output:** ""

**Explanation:** There is no common prefix among the input strings. 

**Constraints:**

*   `1 <= strs.length <= 200`
*   `0 <= strs[i].length <= 200`
*   `strs[i]` consists of only lower-case English letters.

## Solution

```kotlin
class Solution {
    fun longestCommonPrefix(strs: Array<String>): String {
        if (strs.isEmpty()) {
            return ""
        }
        if (strs.size == 1) {
            return strs[0]
        }
        var temp = strs[0]
        var i = 1
        var cur: String
        while (temp.length > 0 && i < strs.size) {
            if (temp.length > strs[i].length) {
                temp = temp.substring(0, strs[i].length)
            }
            cur = strs[i].substring(0, temp.length)
            if (cur != temp) {
                temp = temp.substring(0, temp.length - 1)
            } else {
                i++
            }
        }
        return temp
    }
}
```