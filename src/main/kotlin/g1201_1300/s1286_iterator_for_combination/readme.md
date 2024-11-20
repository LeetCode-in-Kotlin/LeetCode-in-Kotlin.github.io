[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1286\. Iterator for Combination

Medium

Design the `CombinationIterator` class:

*   `CombinationIterator(string characters, int combinationLength)` Initializes the object with a string `characters` of **sorted distinct** lowercase English letters and a number `combinationLength` as arguments.
*   `next()` Returns the next combination of length `combinationLength` in **lexicographical order**.
*   `hasNext()` Returns `true` if and only if there exists a next combination.

**Example 1:**

**Input** ["CombinationIterator", "next", "hasNext", "next", "hasNext", "next", "hasNext"] [["abc", 2], [], [], [], [], [], []]

**Output:** [null, "ab", true, "ac", true, "bc", false]

**Explanation:** CombinationIterator itr = new CombinationIterator("abc", 2); itr.next(); // return "ab" itr.hasNext(); // return True itr.next(); // return "ac" itr.hasNext(); // return True itr.next(); // return "bc" itr.hasNext(); // return False

**Constraints:**

*   `1 <= combinationLength <= characters.length <= 15`
*   All the characters of `characters` are **unique**.
*   At most <code>10<sup>4</sup></code> calls will be made to `next` and `hasNext`.
*   It is guaranteed that all calls of the function `next` are valid.

## Solution

```kotlin
class CombinationIterator(characters: String, private val combinationLength: Int) {
    private val list: MutableList<String>
    private var index = 0
    private val visited: BooleanArray

    init {
        list = ArrayList()
        visited = BooleanArray(characters.length)
        buildAllCombinations(characters, 0, StringBuilder(), visited)
    }

    private fun buildAllCombinations(
        characters: String,
        start: Int,
        sb: StringBuilder,
        visited: BooleanArray,
    ) {
        if (sb.length == combinationLength) {
            list.add(sb.toString())
        } else {
            var i = start
            while (i < characters.length) {
                if (!visited[i]) {
                    sb.append(characters[i])
                    visited[i] = true
                    buildAllCombinations(characters, i++, sb, visited)
                    visited[i - 1] = false
                    sb.setLength(sb.length - 1)
                } else {
                    i++
                }
            }
        }
    }

    operator fun next(): String {
        return list[index++]
    }

    operator fun hasNext(): Boolean {
        return index < list.size
    }
}
/*
 * Your CombinationIterator object will be instantiated and called as such:
 * var obj = CombinationIterator(characters, combinationLength)
 * var param_1 = obj.next()
 * var param_2 = obj.hasNext()
 */
```