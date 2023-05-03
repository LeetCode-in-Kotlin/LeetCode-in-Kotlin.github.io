[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 211\. Design Add and Search Words Data Structure

Medium

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

*   `WordDictionary()` Initializes the object.
*   `void addWord(word)` Adds `word` to the data structure, it can be matched later.
*   `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

**Example:**

**Input**

    ["WordDictionary","addWord","addWord","addWord","search","search","search","search"] [[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]

**Output**

    [null,null,null,null,false,true,true,true]

**Explanation**

    WordDictionary wordDictionary = new WordDictionary();
    wordDictionary.addWord("bad");
    wordDictionary.addWord("dad");
    wordDictionary.addWord("mad");
    wordDictionary.search("pad"); // return False
    wordDictionary.search("bad"); // return True
    wordDictionary.search(".ad"); // return True
    wordDictionary.search("b.."); // return True 

**Constraints:**

*   `1 <= word.length <= 500`
*   `word` in `addWord` consists lower-case English letters.
*   `word` in `search` consist of `'.'` or lower-case English letters.
*   At most `50000` calls will be made to `addWord` and `search`.

## Solution

```kotlin
class WordDictionary {
    class TrieNode() {
        val children = Array<TrieNode?>(26) { null }
        var isWord = false
    }

    val trieTree = TrieNode()

    fun addWord(word: String) {
        var p = trieTree
        for (w in word) {
            val i = w - 'a'
            if (p.children[i] == null) p.children[i] = TrieNode()
            p = p.children[i]!!
        }
        p.isWord = true
    }

    fun search(word: String): Boolean {
        fun dfs(p: TrieNode?, start: Int): Boolean {
            if (p == null) return false
            if (start == word.length) return p.isWord
            return if (word[start] == '.') {
                for (i in 0..25) {
                    if (dfs(p.children[i], start + 1)) {
                        return true
                    }
                }
                false
            } else {
                val i = word[start] - 'a'
                dfs(p.children[i], start + 1)
            }
        }
        return dfs(trieTree, 0)
    }
}

/*
 * Your WordDictionary object will be instantiated and called as such:
 * var obj = WordDictionary()
 * obj.addWord(word)
 * var param_2 = obj.search(word)
 */
```