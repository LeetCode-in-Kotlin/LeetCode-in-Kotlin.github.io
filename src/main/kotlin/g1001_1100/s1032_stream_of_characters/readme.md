[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1032\. Stream of Characters

Hard

Design an algorithm that accepts a stream of characters and checks if a suffix of these characters is a string of a given array of strings `words`.

For example, if `words = ["abc", "xyz"]` and the stream added the four characters (one by one) `'a'`, `'x'`, `'y'`, and `'z'`, your algorithm should detect that the suffix `"xyz"` of the characters `"axyz"` matches `"xyz"` from `words`.

Implement the `StreamChecker` class:

*   `StreamChecker(String[] words)` Initializes the object with the strings array `words`.
*   `boolean query(char letter)` Accepts a new character from the stream and returns `true` if any non-empty suffix from the stream forms a word that is in `words`.

**Example 1:**

**Input** ["StreamChecker", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query"] [[["cd", "f", "kl"]], ["a"], ["b"], ["c"], ["d"], ["e"], ["f"], ["g"], ["h"], ["i"], ["j"], ["k"], ["l"]]

**Output:** [null, false, false, false, true, false, true, false, false, false, false, false, true]

**Explanation:** 

    StreamChecker streamChecker = new StreamChecker(["cd", "f", "kl"]); 
    streamChecker.query("a"); // return False 
    streamChecker.query("b"); // return False 
    streamChecker.query("c"); // return False 
    streamChecker.query("d"); // return True, because 'cd' is in the wordlist 
    streamChecker.query("e"); // return False 
    streamChecker.query("f"); // return True, because 'f' is in the wordlist 
    streamChecker.query("g"); // return False 
    streamChecker.query("h"); // return False 
    streamChecker.query("i"); // return False 
    streamChecker.query("j"); // return False 
    streamChecker.query("k"); // return False 
    streamChecker.query("l"); // return True, because 'kl' is in the wordlist

**Constraints:**

*   `1 <= words.length <= 2000`
*   `1 <= words[i].length <= 2000`
*   `words[i]` consists of lowercase English letters.
*   `letter` is a lowercase English letter.
*   At most <code>4 * 10<sup>4</sup></code> calls will be made to query.

## Solution

```kotlin
class StreamChecker(words: Array<String>) {
    internal class Node {
        var child: Array<Node?> = arrayOfNulls(26)
        var isEnd = false
    }

    private val sb: StringBuilder = StringBuilder()
    private val root: Node = Node()
    fun insert(s: String) {
        var curr: Node? = root
        for (i in s.length - 1 downTo 0) {
            val c = s[i]
            if (curr!!.child[c.code - 'a'.code] == null) {
                curr.child[c.code - 'a'.code] = Node()
            }
            curr = curr.child[c.code - 'a'.code]
        }
        curr!!.isEnd = true
    }

    init {
        for (s in words) {
            insert(s)
        }
    }

    fun query(letter: Char): Boolean {
        sb.append(letter)
        var curr: Node? = root
        for (i in sb.length - 1 downTo 0) {
            val c = sb[i]
            if (curr!!.child[c.code - 'a'.code] == null) {
                return false
            }
            if (curr.child[c.code - 'a'.code]!!.isEnd) {
                return true
            }
            curr = curr.child[c.code - 'a'.code]
        }
        return false
    }
}

/*
 * Your StreamChecker object will be instantiated and called as such:
 * var obj = StreamChecker(words)
 * var param_1 = obj.query(letter)
 */
```