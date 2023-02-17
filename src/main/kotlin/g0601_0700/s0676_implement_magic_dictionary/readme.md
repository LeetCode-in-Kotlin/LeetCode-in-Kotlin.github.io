[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 676\. Implement Magic Dictionary

Medium

Design a data structure that is initialized with a list of **different** words. Provided a string, you should determine if you can change exactly one character in this string to match any word in the data structure.

Implement the `MagicDictionary` class:

*   `MagicDictionary()` Initializes the object.
*   `void buildDict(String[] dictionary)` Sets the data structure with an array of distinct strings `dictionary`.
*   `bool search(String searchWord)` Returns `true` if you can change **exactly one character** in `searchWord` to match any string in the data structure, otherwise returns `false`.

**Example 1:**

**Input**

["MagicDictionary", "buildDict", "search", "search", "search", "search"] [[],

[["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]

**Output:** [null, null, false, true, false, false]

**Explanation:**

    MagicDictionary magicDictionary = new MagicDictionary(); 
    magicDictionary.buildDict(["hello", "leetcode"]); 
    magicDictionary.search("hello"); // return False 
    magicDictionary.search("hhllo"); // We can change the second 'h' to 'e' to match "hello" so we return True 
    magicDictionary.search("hell"); // return False 
    magicDictionary.search("leetcoded"); // return False

**Constraints:**

*   `1 <= dictionary.length <= 100`
*   `1 <= dictionary[i].length <= 100`
*   `dictionary[i]` consists of only lower-case English letters.
*   All the strings in `dictionary` are **distinct**.
*   `1 <= searchWord.length <= 100`
*   `searchWord` consists of only lower-case English letters.
*   `buildDict` will be called only once before `search`.
*   At most `100` calls will be made to `search`.

## Solution

```kotlin
class MagicDictionary {
    private var dictionaryWords: Array<String>? = null
    fun buildDict(dictionary: Array<String>?) {
        dictionaryWords = dictionary
    }

    fun search(searchWord: String): Boolean {
        for (word in dictionaryWords!!) {
            if (isOffByOneLetter(word, searchWord)) {
                return true
            }
        }
        return false
    }

    private fun isOffByOneLetter(word: String, searchWord: String): Boolean {
        if (isDifferentLengths(word, searchWord) || word == searchWord) {
            return false
        }
        var numDifferentLetters = 0
        for (i in word.indices) {
            if (isNotTheSameLetter(word[i], searchWord[i])) {
                numDifferentLetters++
            }
            if (numDifferentLetters > 1) {
                return false
            }
        }
        return numDifferentLetters == 1
    }

    private fun isDifferentLengths(word: String, searchWord: String): Boolean {
        return word.length != searchWord.length
    }

    private fun isNotTheSameLetter(c1: Char, c2: Char): Boolean {
        return c1 != c2
    }
}

/*
 * Your MagicDictionary object will be instantiated and called as such:
 * var obj = MagicDictionary()
 * obj.buildDict(dictionary)
 * var param_2 = obj.search(searchWord)
 */
```