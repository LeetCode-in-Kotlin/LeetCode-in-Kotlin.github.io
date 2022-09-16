[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 763\. Partition Labels

Medium

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return _a list of integers representing the size of these parts_.

**Example 1:**

**Input:** s = "ababcbacadefegdehijhklij"

**Output:** [9,7,8]

**Explanation:** The partition is "ababcbaca", "defegde", "hijhklij". This is a partition so that each letter appears in at most one part. A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.

**Example 2:**

**Input:** s = "eccbbbbdec"

**Output:** [10]

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun partitionLabels(s: String): List<Int> {
        val letters = s.toCharArray()
        val result: MutableList<Int> = ArrayList()
        val position = IntArray(26)
        for (i in letters.indices) {
            position[letters[i] - 'a'] = i
        }
        var i = 0
        var prev = -1
        var max = 0
        while (i < letters.size) {
            if (position[letters[i] - 'a'] > max) {
                max = position[letters[i] - 'a']
            }
            if (i == max) {
                result.add(i - prev)
                prev = i
            }
            i++
        }
        return result
    }
}
```