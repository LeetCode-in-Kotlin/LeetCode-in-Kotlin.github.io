[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 955\. Delete Columns to Make Sorted II

Medium

You are given an array of `n` strings `strs`, all of the same length.

We may choose any deletion indices, and we delete all the characters in those indices for each string.

For example, if we have `strs = ["abcdef","uvwxyz"]` and deletion indices `{0, 2, 3}`, then the final array after deletions is `["bef", "vyz"]`.

Suppose we chose a set of deletion indices `answer` such that after deletions, the final array has its elements in **lexicographic** order (i.e., `strs[0] <= strs[1] <= strs[2] <= ... <= strs[n - 1]`). Return _the minimum possible value of_ `answer.length`.

**Example 1:**

**Input:** strs = ["ca","bb","ac"]

**Output:** 1

**Explanation:** After deleting the first column, strs = ["a", "b", "c"]. Now strs is in lexicographic order (ie. strs[0] <= strs[1] <= strs[2]). We require at least 1 deletion since initially strs was not in lexicographic order, so the answer is 1.

**Example 2:**

**Input:** strs = ["xc","yb","za"]

**Output:** 0

**Explanation:** strs is already in lexicographic order, so we do not need to delete anything. Note that the rows of strs are not necessarily in lexicographic order: i.e., it is NOT necessarily true that (strs[0][0] <= strs[0][1] <= ...)

**Example 3:**

**Input:** strs = ["zyx","wvu","tsr"]

**Output:** 3

**Explanation:** We have to delete every column.

**Constraints:**

*   `n == strs.length`
*   `1 <= n <= 100`
*   `1 <= strs[i].length <= 100`
*   `strs[i]` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minDeletionSize(strs: Array<String>): Int {
        val sorted = BooleanArray(strs.size)
        var res = 0
        for (i in 0 until strs[0].length) {
            var j = 0
            while (j < strs.size - 1) {
                if (!sorted[j] && strs[j][i] > strs[j + 1][i]) {
                    res++
                    break
                }
                j++
            }
            if (j < strs.size - 1) {
                continue
            }
            j = 0
            while (j < strs.size - 1) {
                if (strs[j][i] < strs[j + 1][i]) {
                    sorted[j] = true
                }
                j++
            }
        }
        return res
    }
}
```