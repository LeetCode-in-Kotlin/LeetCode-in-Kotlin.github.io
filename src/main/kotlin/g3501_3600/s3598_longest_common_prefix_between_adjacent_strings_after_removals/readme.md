[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3598\. Longest Common Prefix Between Adjacent Strings After Removals

Medium

You are given an array of strings `words`. For each index `i` in the range `[0, words.length - 1]`, perform the following steps:

*   Remove the element at index `i` from the `words` array.
*   Compute the **length** of the **longest common prefix** among all **adjacent** pairs in the modified array.

Return an array `answer`, where `answer[i]` is the length of the longest common prefix between the adjacent pairs after removing the element at index `i`. If **no** adjacent pairs remain or if **none** share a common prefix, then `answer[i]` should be 0.

**Example 1:**

**Input:** words = ["jump","run","run","jump","run"]

**Output:** [3,0,0,3,3]

**Explanation:**

*   Removing index 0:
    *   `words` becomes `["run", "run", "jump", "run"]`
    *   Longest adjacent pair is `["run", "run"]` having a common prefix `"run"` (length 3)
*   Removing index 1:
    *   `words` becomes `["jump", "run", "jump", "run"]`
    *   No adjacent pairs share a common prefix (length 0)
*   Removing index 2:
    *   `words` becomes `["jump", "run", "jump", "run"]`
    *   No adjacent pairs share a common prefix (length 0)
*   Removing index 3:
    *   `words` becomes `["jump", "run", "run", "run"]`
    *   Longest adjacent pair is `["run", "run"]` having a common prefix `"run"` (length 3)
*   Removing index 4:
    *   words becomes `["jump", "run", "run", "jump"]`
    *   Longest adjacent pair is `["run", "run"]` having a common prefix `"run"` (length 3)

**Example 2:**

**Input:** words = ["dog","racer","car"]

**Output:** [0,0,0]

**Explanation:**

*   Removing any index results in an answer of 0.

**Constraints:**

*   <code>1 <= words.length <= 10<sup>5</sup></code>
*   <code>1 <= words[i].length <= 10<sup>4</sup></code>
*   `words[i]` consists of lowercase English letters.
*   The sum of `words[i].length` is smaller than or equal <code>10<sup>5</sup></code>.

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    private fun solve(a: String, b: String): Int {
        val len = min(a.length, b.length)
        var cnt = 0
        while (cnt < len && a[cnt] == b[cnt]) {
            cnt++
        }
        return cnt
    }

    fun longestCommonPrefix(words: Array<String>): IntArray {
        val n = words.size
        val ans = IntArray(n)
        if (n <= 1) {
            return ans
        }
        val lcp = IntArray(n - 1)
        run {
            var i = 0
            while (i + 1 < n) {
                lcp[i] = solve(words[i], words[i + 1])
                i++
            }
        }
        val prefmax = IntArray(n - 1)
        val sufmax = IntArray(n - 1)
        prefmax[0] = lcp[0]
        for (i in 1..<n - 1) {
            prefmax[i] = max(prefmax[i - 1], lcp[i])
        }
        sufmax[n - 2] = lcp[n - 2]
        for (i in n - 3 downTo 0) {
            sufmax[i] = max(sufmax[i + 1], lcp[i])
        }
        for (i in 0..<n) {
            var best = 0
            if (i >= 2) {
                best = max(best, prefmax[i - 2])
            }
            if (i + 1 <= n - 2) {
                best = max(best, sufmax[i + 1])
            }
            if (i > 0 && i < n - 1) {
                best = max(best, solve(words[i - 1], words[i + 1]))
            }
            ans[i] = best
        }
        return ans
    }
}
```