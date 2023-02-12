[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 648\. Replace Words

Medium

In English, we have a concept called **root**, which can be followed by some other word to form another longer word - let's call this word **successor**. For example, when the **root** `"an"` is followed by the **successor** word `"other"`, we can form a new word `"another"`.

Given a `dictionary` consisting of many **roots** and a `sentence` consisting of words separated by spaces, replace all the **successors** in the sentence with the **root** forming it. If a **successor** can be replaced by more than one **root**, replace it with the **root** that has **the shortest length**.

Return _the `sentence`_ after the replacement.

**Example 1:**

**Input:** dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"

**Output:** "the cat was rat by the bat"

**Example 2:**

**Input:** dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"

**Output:** "a a b c"

**Constraints:**

*   `1 <= dictionary.length <= 1000`
*   `1 <= dictionary[i].length <= 100`
*   `dictionary[i]` consists of only lower-case letters.
*   <code>1 <= sentence.length <= 10<sup>6</sup></code>
*   `sentence` consists of only lower-case letters and spaces.
*   The number of words in `sentence` is in the range `[1, 1000]`
*   The length of each word in `sentence` is in the range `[1, 1000]`
*   Every two consecutive words in `sentence` will be separated by exactly one space.
*   `sentence` does not have leading or trailing spaces.

## Solution

```kotlin
import java.util.function.Consumer

class Solution {
    fun replaceWords(dictionary: List<String>, sentence: String): String {
        val trie = Trie()
        dictionary.forEach(Consumer { word: String -> trie.insert(word) })
        val allWords = sentence.split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        for (i in allWords.indices) {
            allWords[i] = trie.getRootForWord(allWords[i])
        }
        return java.lang.String.join(" ", *allWords)
    }

    internal class Node {
        var links = arrayOfNulls<Node>(26)
        var isWordCompleted = false

        fun containsKey(ch: Char): Boolean {
            return links[ch.code - 'a'.code] != null
        }

        fun put(ch: Char, node: Node?) {
            links[ch.code - 'a'.code] = node
        }

        operator fun get(ch: Char): Node? {
            return links[ch.code - 'a'.code]
        }
    }

    internal class Trie {
        var root: Node = Node()

        fun insert(word: String) {
            var node: Node? = root
            for (i in word.indices) {
                if (!node!!.containsKey(word[i])) {
                    node.put(word[i], Node())
                }
                node = node[word[i]]
            }
            node!!.isWordCompleted = true
        }

        fun getRootForWord(word: String): String {
            var node: Node? = root
            val rootWord = StringBuilder()
            for (i in word.indices) {
                if (node!!.containsKey(word[i])) {
                    rootWord.append(word[i])
                    node = node[word[i]]
                    if (node!!.isWordCompleted) {
                        return rootWord.toString()
                    }
                } else {
                    return word
                }
            }
            return word
        }
    }
}
```