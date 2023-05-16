[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 966\. Vowel Spellchecker

Medium

Given a `wordlist`, we want to implement a spellchecker that converts a query word into a correct word.

For a given `query` word, the spell checker handles two categories of spelling mistakes:

*   Capitalization: If the query matches a word in the wordlist (**case-insensitive**), then the query word is returned with the same case as the case in the wordlist.
    *   Example: `wordlist = ["yellow"]`, `query = "YellOw"`: `correct = "yellow"`
    *   Example: `wordlist = ["Yellow"]`, `query = "yellow"`: `correct = "Yellow"`
    *   Example: `wordlist = ["yellow"]`, `query = "yellow"`: `correct = "yellow"`
*   Vowel Errors: If after replacing the vowels `('a', 'e', 'i', 'o', 'u')` of the query word with any vowel individually, it matches a word in the wordlist (**case-insensitive**), then the query word is returned with the same case as the match in the wordlist.
    *   Example: `wordlist = ["YellOw"]`, `query = "yollow"`: `correct = "YellOw"`
    *   Example: `wordlist = ["YellOw"]`, `query = "yeellow"`: `correct = ""` (no match)
    *   Example: `wordlist = ["YellOw"]`, `query = "yllw"`: `correct = ""` (no match)

In addition, the spell checker operates under the following precedence rules:

*   When the query exactly matches a word in the wordlist (**case-sensitive**), you should return the same word back.
*   When the query matches a word up to capitlization, you should return the first such match in the wordlist.
*   When the query matches a word up to vowel errors, you should return the first such match in the wordlist.
*   If the query has no matches in the wordlist, you should return the empty string.

Given some `queries`, return a list of words `answer`, where `answer[i]` is the correct word for `query = queries[i]`.

**Example 1:**

**Input:** wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]

**Output:** ["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]

**Example 2:**

**Input:** wordlist = ["yellow"], queries = ["YellOw"]

**Output:** ["yellow"]

**Constraints:**

*   `1 <= wordlist.length, queries.length <= 5000`
*   `1 <= wordlist[i].length, queries[i].length <= 7`
*   `wordlist[i]` and `queries[i]` consist only of only English letters.

## Solution

```kotlin
class Solution {
    private var matched: HashSet<String>? = null
    private var capitalizations: HashMap<String, String>? = null
    private var vowelErrors: HashMap<String, String>? = null
    private fun isVowel(w: Char): Boolean {
        return w == 'a' || w == 'e' || w == 'i' || w == 'o' || w == 'u'
    }

    private fun removeVowels(word: String): String {
        val s = StringBuilder()
        for (w in word.toCharArray()) {
            s.append(if (isVowel(w)) '*' else w)
        }
        return s.toString()
    }

    private fun solveQuery(query: String): String? {
        if (matched!!.contains(query)) {
            return query
        }
        var word = query.lowercase()
        if (capitalizations!!.containsKey(word)) {
            return capitalizations!![word]
        }
        word = removeVowels(word)
        return if (vowelErrors!!.containsKey(word)) {
            vowelErrors!![word]
        } else ""
    }

    fun spellchecker(wordlist: Array<String>, queries: Array<String>): Array<String?> {
        val answer = arrayOfNulls<String>(queries.size)
        matched = HashSet()
        capitalizations = HashMap()
        vowelErrors = HashMap()
        for (word in wordlist) {
            matched!!.add(word)
            var s = word.lowercase()
            capitalizations!!.putIfAbsent(s, word)
            s = removeVowels(s)
            vowelErrors!!.putIfAbsent(s, word)
        }
        for (i in queries.indices) {
            answer[i] = solveQuery(queries[i])
        }
        return answer
    }
}
```