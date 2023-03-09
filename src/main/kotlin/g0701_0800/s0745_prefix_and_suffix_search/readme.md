[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 745\. Prefix and Suffix Search

Hard

Design a special dictionary that searches the words in it by a prefix and a suffix.

Implement the `WordFilter` class:

*   `WordFilter(string[] words)` Initializes the object with the `words` in the dictionary.
*   `f(string pref, string suff)` Returns _the index of the word in the dictionary,_ which has the prefix `pref` and the suffix `suff`. If there is more than one valid index, return **the largest** of them. If there is no such word in the dictionary, return `-1`.

**Example 1:**

**Input** ["WordFilter", "f"] [[["apple"]], ["a", "e"]]

**Output:** [null, 0]

**Explanation:** WordFilter wordFilter = new WordFilter(["apple"]); wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = "e".

**Constraints:**

*   <code>1 <= words.length <= 10<sup>4</sup></code>
*   `1 <= words[i].length <= 7`
*   `1 <= pref.length, suff.length <= 7`
*   `words[i]`, `pref` and `suff` consist of lowercase English letters only.
*   At most <code>10<sup>4</sup></code> calls will be made to the function `f`.

## Solution

```kotlin
class WordFilter(words: Array<String>) {
    private class TrieNode {
        var children: Array<TrieNode?> = arrayOfNulls(27)
        var weight: Int = 0
    }

    private val root = TrieNode()

    init {
        for (i in words.indices) {
            val s = words[i]
            for (j in s.indices) {
                insert(s.substring(j) + '{' + s, i)
            }
        }
    }

    private fun insert(wd: String, weight: Int) {
        var node: TrieNode? = root
        for (ch in wd.toCharArray()) {
            if (node!!.children[ch.code - 'a'.code] == null) {
                node.children[ch.code - 'a'.code] = TrieNode()
            }
            node = node.children[ch.code - 'a'.code]
            node!!.weight = weight
        }
    }

    fun f(prefix: String, suffix: String): Int {
        return startsWith("$suffix{$prefix")
    }

    private fun startsWith(pref: String): Int {
        var node: TrieNode? = root
        for (ch in pref.toCharArray()) {
            if (node!!.children[ch.code - 'a'.code] == null) {
                return -1
            }
            node = node.children[ch.code - 'a'.code]
        }
        return node!!.weight
    }
}

/*
 * Your WordFilter object will be instantiated and called as such:
 * var obj = WordFilter(words)
 * var param_1 = obj.f(pref,suff)
 */
```