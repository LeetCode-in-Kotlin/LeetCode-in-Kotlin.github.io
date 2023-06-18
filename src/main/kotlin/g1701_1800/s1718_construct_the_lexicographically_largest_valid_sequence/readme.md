[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1718\. Construct the Lexicographically Largest Valid Sequence

Medium

Given an integer `n`, find a sequence that satisfies all of the following:

*   The integer `1` occurs once in the sequence.
*   Each integer between `2` and `n` occurs twice in the sequence.
*   For every integer `i` between `2` and `n`, the **distance** between the two occurrences of `i` is exactly `i`.

The **distance** between two numbers on the sequence, `a[i]` and `a[j]`, is the absolute difference of their indices, `|j - i|`.

Return _the **lexicographically largest** sequence__. It is guaranteed that under the given constraints, there is always a solution._

A sequence `a` is lexicographically larger than a sequence `b` (of the same length) if in the first position where `a` and `b` differ, sequence `a` has a number greater than the corresponding number in `b`. For example, `[0,1,9,0]` is lexicographically larger than `[0,1,5,6]` because the first position they differ is at the third number, and `9` is greater than `5`.

**Example 1:**

**Input:** n = 3

**Output:** [3,1,2,3,2]

**Explanation:** [2,3,2,1,3] is also a valid sequence, but [3,1,2,3,2] is the lexicographically largest valid sequence.

**Example 2:**

**Input:** n = 5

**Output:** [5,3,1,4,3,5,2,4,2]

**Constraints:**

*   `1 <= n <= 20`

## Solution

```kotlin
class Solution {
    fun constructDistancedSequence(n: Int): IntArray {
        val result = IntArray(n * 2 - 1)
        val visited = BooleanArray(n + 1)
        backtracking(0, result, visited, n)
        return result
    }

    private fun backtracking(index: Int, result: IntArray, visited: BooleanArray, n: Int): Boolean {
        if (index == result.size) {
            return true
        }
        if (result[index] != 0) {
            return backtracking(index + 1, result, visited, n)
        } else {
            for (i in n downTo 1) {
                if (visited[i]) {
                    continue
                }
                visited[i] = true
                result[index] = i
                if (i == 1) {
                    if (backtracking(index + 1, result, visited, n)) {
                        return true
                    }
                } else if (index + i < result.size && result[index + i] == 0) {
                    result[i + index] = i
                    if (backtracking(index + 1, result, visited, n)) {
                        return true
                    }
                    result[index + i] = 0
                }
                result[index] = 0
                visited[i] = false
            }
        }
        return false
    }
}
```