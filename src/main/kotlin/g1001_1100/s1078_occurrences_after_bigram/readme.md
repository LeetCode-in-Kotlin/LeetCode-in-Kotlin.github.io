[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1078\. Occurrences After Bigram

Easy

Given two strings `first` and `second`, consider occurrences in some text of the form `"first second third"`, where `second` comes immediately after `first`, and `third` comes immediately after `second`.

Return _an array of all the words_ `third` _for each occurrence of_ `"first second third"`.

**Example 1:**

**Input:** text = "alice is a good girl she is a good student", first = "a", second = "good"

**Output:** ["girl","student"]

**Example 2:**

**Input:** text = "we will we will rock you", first = "we", second = "will"

**Output:** ["we","rock"]

**Constraints:**

*   `1 <= text.length <= 1000`
*   `text` consists of lowercase English letters and spaces.
*   All the words in `text` a separated by **a single space**.
*   `1 <= first.length, second.length <= 10`
*   `first` and `second` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun findOcurrences(text: String, first: String, second: String): Array<String?> {
        val list: MutableList<String> = ArrayList()
        val str = text.split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        for (i in str.indices) {
            if (str[i] == first && str.size - 1 >= i + 2 && str[i + 1] == second) {
                list.add(str[i + 2])
            }
        }
        val s = arrayOfNulls<String>(list.size)
        var j = 0
        for (ele in list) {
            s[j++] = ele
        }
        return s
    }
}
```