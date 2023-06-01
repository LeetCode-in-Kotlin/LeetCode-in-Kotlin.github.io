[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1079\. Letter Tile Possibilities

Medium

You have `n` `tiles`, where each tile has one letter `tiles[i]` printed on it.

Return _the number of possible non-empty sequences of letters_ you can make using the letters printed on those `tiles`.

**Example 1:**

**Input:** tiles = "AAB"

**Output:** 8

**Explanation:** The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

**Example 2:**

**Input:** tiles = "AAABBC"

**Output:** 188

**Example 3:**

**Input:** tiles = "V"

**Output:** 1

**Constraints:**

*   `1 <= tiles.length <= 7`
*   `tiles` consists of uppercase English letters.

## Solution

```kotlin
class Solution {
    private var count = 0

    fun numTilePossibilities(tiles: String): Int {
        count = 0
        val chars = tiles.toCharArray()
        chars.sort()
        val visited = BooleanArray(chars.size)
        dfs(chars, 0, visited)
        return count
    }

    private fun dfs(chars: CharArray, length: Int, visited: BooleanArray) {
        if (length == chars.size) {
            return
        }
        for (i in chars.indices) {
            if (visited[i] || i - 1 >= 0 && chars[i] == chars[i - 1] && !visited[i - 1]) {
                continue
            }
            count++
            visited[i] = true
            dfs(chars, length + 1, visited)
            visited[i] = false
        }
    }
}
```