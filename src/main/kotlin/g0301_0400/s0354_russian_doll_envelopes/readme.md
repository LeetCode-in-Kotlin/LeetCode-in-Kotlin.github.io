[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 354\. Russian Doll Envelopes

Hard

You are given a 2D array of integers `envelopes` where <code>envelopes[i] = [w<sub>i</sub>, h<sub>i</sub>]</code> represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return _the maximum number of envelopes you can Russian doll (i.e., put one inside the other)_.

**Note:** You cannot rotate an envelope.

**Example 1:**

**Input:** envelopes = \[\[5,4],[6,4],[6,7],[2,3]]

**Output:** 3

**Explanation:** The maximum number of envelopes you can Russian doll is `3` ([2,3] => [5,4] => [6,7]).

**Example 2:**

**Input:** envelopes = \[\[1,1],[1,1],[1,1]]

**Output:** 1

**Constraints:**

*   <code>1 <= envelopes.length <= 10<sup>5</sup></code>
*   `envelopes[i].length == 2`
*   <code>1 <= w<sub>i</sub>, h<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun maxEnvelopes(envelopes: Array<IntArray>): Int {
        envelopes.sortWith { a: IntArray, b: IntArray ->
            if (a[0] != b[0]
            ) a[0] - b[0] else b[1] - a[1]
        }
        val tails = IntArray(envelopes.size)
        var size = 0
        for (enve in envelopes) {
            var i = 0
            var j = size
            while (i != j) {
                val mid = i + (j - i shr 1)
                if (tails[mid] < enve[1]) {
                    i = mid + 1
                } else {
                    j = mid
                }
            }
            tails[i] = enve[1]
            if (i == size) {
                size++
            }
        }
        return size
    }
}
```