[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 208\. Implement Trie (Prefix Tree)

Medium

A [**trie**](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

*   `Trie()` Initializes the trie object.
*   `void insert(String word)` Inserts the string `word` into the trie.
*   `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
*   `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

**Example 1:**

**Input** ["Trie", "insert", "search", "search", "startsWith", "insert", "search"] [[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]

**Output:** [null, null, true, false, true, null, true]

**Explanation:** 

Trie trie = new Trie(); 

trie.insert("apple"); 

trie.search("apple"); // return True 

trie.search("app"); // return False 

trie.startsWith("app"); // return True 

trie.insert("app"); 

trie.search("app"); // return True

**Constraints:**

*   `1 <= word.length, prefix.length <= 2000`
*   `word` and `prefix` consist only of lowercase English letters.
*   At most <code>3 * 10<sup>4</sup></code> calls **in total** will be made to `insert`, `search`, and `startsWith`.

## Solution

```kotlin
class Trie {
    private val root: TrieNode
    private var startWith = false

    class TrieNode {
        // Initialize your data structure here.
        var children: Array<TrieNode?>
        var isWord = false

        init {
            children = arrayOfNulls(26)
        }
    }

    // Inserts a word into the trie.
    fun insert(word: String) {
        insert(word, root, 0)
    }

    private fun insert(word: String, root: TrieNode?, idx: Int) {
        if (idx == word.length) {
            root!!.isWord = true
            return
        }
        val index = word[idx] - 'a'
        if (root!!.children[index] == null) {
            root.children[index] = TrieNode()
        }
        insert(word, root.children[index], idx + 1)
    }

    // Returns if the word is in the trie.
    @JvmOverloads
    fun search(word: String, root: TrieNode? = this.root, idx: Int = 0): Boolean {
        if (idx == word.length) {
            startWith = true
            return root!!.isWord
        }
        val index = word[idx] - 'a'
        if (root!!.children[index] == null) {
            startWith = false
            return false
        }
        return search(word, root.children[index], idx + 1)
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    fun startsWith(prefix: String): Boolean {
        search(prefix)
        return startWith
    }

    init {
        root = TrieNode()
    }
}
```