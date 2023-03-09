[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 51\. N-Queens

Hard

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4

**Output:** [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]

**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above

**Example 2:**

**Input:** n = 1

**Output:** [["Q"]]

**Constraints:**

*   `1 <= n <= 9`

## Solution

```kotlin
class Solution {
    fun solveNQueens(n: Int): List<List<String>> {
        val pos = BooleanArray(n + 2 * n - 1 + 2 * n - 1)
        val pos2 = IntArray(n)
        val ans: MutableList<List<String>> = ArrayList()
        helper(n, 0, pos, pos2, ans)
        return ans
    }

    private fun helper(n: Int, row: Int, pos: BooleanArray, pos2: IntArray, ans: MutableList<List<String>>) {
        if (row == n) {
            construct(n, pos2, ans)
            return
        }
        for (i in 0 until n) {
            val index = n + 2 * n - 1 + n - 1 + i - row
            if (pos[i] || pos[n + i + row] || pos[index]) {
                continue
            }
            pos[i] = true
            pos[n + i + row] = true
            pos[index] = true
            pos2[row] = i
            helper(n, row + 1, pos, pos2, ans)
            pos[i] = false
            pos[n + i + row] = false
            pos[index] = false
        }
    }

    private fun construct(n: Int, pos: IntArray, ans: MutableList<List<String>>) {
        val sol: MutableList<String> = ArrayList()
        for (r in 0 until n) {
            val queenRow = CharArray(n)
            queenRow.fill('.')
            queenRow[pos[r]] = 'Q'
            sol.add(String(queenRow))
        }
        ans.add(sol)
    }
}
```