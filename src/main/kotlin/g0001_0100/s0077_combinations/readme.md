[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 77\. Combinations

Medium

Given two integers `n` and `k`, return _all possible combinations of_ `k` _numbers chosen from the range_ `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

**Input:** n = 4, k = 2

**Output:** [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]

**Explanation:** There are 4 choose 2 = 6 total combinations. Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.

**Example 2:**

**Input:** n = 1, k = 1

**Output:** [[1]]

**Explanation:** There is 1 choose 1 = 1 total combination.

**Constraints:**

*   `1 <= n <= 20`
*   `1 <= k <= n`

## Solution

```kotlin
class Solution {
    fun combine(n: Int, k: Int): List<List<Int>> {
        val ans: MutableList<List<Int>> = ArrayList()
        // Boundary case
        if (n > 20 || k < 1 || k > n) {
            return ans
        }
        backtrack(ans, n, k, 1, ArrayDeque())
        return ans
    }

    private fun backtrack(ans: MutableList<List<Int>>, n: Int, k: Int, s: Int, stack: ArrayDeque<Int>) {
        // Base case
        // If k becomes 0
        if (k == 0) {
            ans.add(ArrayList(stack))
            return
        }
        // Start with s till n-k+1
        for (i in s..n - k + 1) {
            stack.addLast(i)
            // Update start for recursion and decrease k by 1
            backtrack(ans, n, k - 1, i + 1, stack)
            stack.removeLast()
        }
    }
}
```