[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 692\. Top K Frequent Words

Medium

Given an array of strings `words` and an integer `k`, return _the_ `k` _most frequent strings_.

Return the answer **sorted** by **the frequency** from highest to lowest. Sort the words with the same frequency by their **lexicographical order**.

**Example 1:**

**Input:** words = ["i","love","leetcode","i","love","coding"], k = 2

**Output:** ["i","love"]

**Explanation:** "i" and "love" are the two most frequent words. Note that "i" comes before "love" due to a lower alphabetical order.

**Example 2:**

**Input:** words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4

**Output:** ["the","is","sunny","day"]

**Explanation:** "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively.

**Constraints:**

*   `1 <= words.length <= 500`
*   `1 <= words[i].length <= 10`
*   `words[i]` consists of lowercase English letters.
*   `k` is in the range <code>[1, The number of **unique** words[i]]</code>

**Follow-up:** Could you solve it in `O(n log(k))` time and `O(n)` extra space?

## Solution

```kotlin
import java.util.SortedSet
import java.util.TreeSet

@Suppress("NAME_SHADOWING")
class Solution {
    fun topKFrequent(words: Array<String>, k: Int): List<String> {
        var k = k
        val map: MutableMap<String, Int> = HashMap()
        for (word in words) {
            map[word] = map.getOrDefault(word, 0) + 1
        }
        val sortedset: SortedSet<Map.Entry<String, Int>> = TreeSet(
            java.util.Comparator { (key, value): Map.Entry<String, Int>, (key1, value1): Map.Entry<String, Int> ->
                return@Comparator if (value != value1) {
                    value1 - value
                } else {
                    key.compareTo(key1, ignoreCase = true)
                }
            },
        )
        sortedset.addAll(map.entries)
        val result: MutableList<String> = ArrayList()
        val iterator: Iterator<Map.Entry<String, Int>> = sortedset.iterator()
        while (iterator.hasNext() && k-- > 0) {
            result.add(iterator.next().key)
        }
        return result
    }
}
```