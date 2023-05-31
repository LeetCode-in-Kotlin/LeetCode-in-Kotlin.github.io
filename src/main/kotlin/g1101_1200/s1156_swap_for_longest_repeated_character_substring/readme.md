[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1156\. Swap For Longest Repeated Character Substring

Medium

You are given a string `text`. You can swap two of the characters in the `text`.

Return _the length of the longest substring with repeated characters_.

**Example 1:**

**Input:** text = "ababa"

**Output:** 3

**Explanation:** We can swap the first 'b' with the last 'a', or the last 'b' with the first 'a'. Then, the longest repeated character substring is "aaa" with length 3.

**Example 2:**

**Input:** text = "aaabaaa"

**Output:** 6

**Explanation:** Swap 'b' with the last 'a' (or the first 'a'), and we get longest repeated character substring "aaaaaa" with length 6.

**Example 3:**

**Input:** text = "aaaaa"

**Output:** 5

**Explanation:** No need to swap, longest repeated character substring is "aaaaa" with length is 5.

**Constraints:**

*   <code>1 <= text.length <= 2 * 10<sup>4</sup></code>
*   `text` consist of lowercase English characters only.

## Solution

```kotlin
class Solution {
    private class Pair(var character: Char, var count: Int)

    fun maxRepOpt1(text: String): Int {
        val pairs: MutableList<Pair> = ArrayList()
        val map: MutableMap<Char, Int> = HashMap()
        // collect counts for each char-block
        var i = 0
        while (i < text.length) {
            val c = text[i]
            var count = 0
            while (i < text.length && text[i] == c) {
                count++
                i++
            }
            pairs.add(Pair(c, count))
            map[c] = map.getOrDefault(c, 0) + count
        }
        var max = 0
        // case 1, swap 1 item to the boundary of a consecutive cha-block to achieve possible max
        // length
        // we need total count to make sure whether a swap is possible!
        for (p in pairs) {
            val totalCount = map.getValue(p.character)
            max = if (totalCount > p.count) {
                Math.max(max, p.count + 1)
            } else {
                Math.max(max, p.count)
            }
        }
        // case 2, find xxxxYxxxxx pattern
        // we need total count to make sure whether a swap is possible!
        for (j in 1 until pairs.size - 1) {
            if (pairs[j - 1].character == pairs[j + 1].character &&
                pairs[j].count == 1
            ) {
                val totalCount = map.getValue(pairs[j - 1].character)
                val groupSum = pairs[j - 1].count + pairs[j + 1].count
                max = if (totalCount > groupSum) {
                    Math.max(max, groupSum + 1)
                } else {
                    Math.max(max, groupSum)
                }
            }
        }
        return max
    }
}
```