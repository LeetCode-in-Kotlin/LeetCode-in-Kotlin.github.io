[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 205\. Isomorphic Strings

Easy

Given two strings `s` and `t`, _determine if they are isomorphic_.

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

**Example 1:**

**Input:** s = "egg", t = "add"

**Output:** true

**Example 2:**

**Input:** s = "foo", t = "bar"

**Output:** false

**Example 3:**

**Input:** s = "paper", t = "title"

**Output:** true

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `t.length == s.length`
*   `s` and `t` consist of any valid ascii character.

## Solution

```kotlin
class Solution {
    fun isIsomorphic(s: String, t: String): Boolean {
        val map = IntArray(128)
        val str = s.toCharArray()
        val tar = t.toCharArray()
        val n = str.size
        for (i in 0 until n) {
            if (map[tar[i].code] == 0) {
                if (search(map, str[i].code, tar[i].code) != -1) {
                    return false
                }
                map[tar[i].code] = str[i].code
            } else {
                if (map[tar[i].code] != str[i].code) {
                    return false
                }
            }
        }
        return true
    }

    private fun search(map: IntArray, tar: Int, skip: Int): Int {
        for (i in 0..127) {
            if (i == skip) {
                continue
            }
            if (map[i] != 0 && map[i] == tar) {
                return i
            }
        }
        return -1
    }
}
```