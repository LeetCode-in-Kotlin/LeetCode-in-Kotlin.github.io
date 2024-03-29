[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1047\. Remove All Adjacent Duplicates In String

Easy

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return _the final string after all such duplicate removals have been made_. It can be proven that the answer is **unique**.

**Example 1:**

**Input:** s = "abbaca"

**Output:** "ca"

**Explanation:** For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move. The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".

**Example 2:**

**Input:** s = "azxxzy"

**Output:** "ay"

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun removeDuplicates(s: String): String {
        if (s.length == 1) {
            return s
        }
        val array = s.toCharArray()
        val length = array.size
        var fast = 0
        var slow = 0
        while (fast < length) {
            if (slow == 0 || array[fast] != array[slow - 1]) {
                array[slow++] = array[fast++]
            } else {
                if (array[fast] == array[slow - 1]) {
                    fast++
                }
                slow--
            }
        }
        return String(array, 0, slow)
    }
}
```