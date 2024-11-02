[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3327\. Check if DFS Strings Are Palindromes

Hard

You are given a tree rooted at node 0, consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by an array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node 0 is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to node `i`.

Consider an empty string `dfsStr`, and define a recursive function `dfs(int x)` that takes a node `x` as a parameter and performs the following steps in order:

*   Iterate over each child `y` of `x` **in increasing order of their numbers**, and call `dfs(y)`.
*   Add the character `s[x]` to the end of the string `dfsStr`.

**Note** that `dfsStr` is shared across all recursive calls of `dfs`.

You need to find a boolean array `answer` of size `n`, where for each index `i` from `0` to `n - 1`, you do the following:

*   Empty the string `dfsStr` and call `dfs(i)`.
*   If the resulting string `dfsStr` is a **palindrome**, then set `answer[i]` to `true`. Otherwise, set `answer[i]` to `false`.

Return the array `answer`.

A **palindrome** is a string that reads the same forward and backward.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/09/01/tree1drawio.png)

**Input:** parent = [-1,0,0,1,1,2], s = "aababa"

**Output:** [true,true,false,true,true,true]

**Explanation:**

*   Calling `dfs(0)` results in the string `dfsStr = "abaaba"`, which is a palindrome.
*   Calling `dfs(1)` results in the string `dfsStr = "aba"`, which is a palindrome.
*   Calling `dfs(2)` results in the string `dfsStr = "ab"`, which is **not** a palindrome.
*   Calling `dfs(3)` results in the string `dfsStr = "a"`, which is a palindrome.
*   Calling `dfs(4)` results in the string `dfsStr = "b"`, which is a palindrome.
*   Calling `dfs(5)` results in the string `dfsStr = "a"`, which is a palindrome.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/09/01/tree2drawio-1.png)

**Input:** parent = [-1,0,0,0,0], s = "aabcb"

**Output:** [true,true,true,true,true]

**Explanation:**

Every call on `dfs(x)` results in a palindrome string.

**Constraints:**

*   `n == parent.length == s.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= parent[i] <= n - 1` for all `i >= 1`.
*   `parent[0] == -1`
*   `parent` represents a valid tree.
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    private val e: MutableList<MutableList<Int?>?> = ArrayList<MutableList<Int?>?>()
    private val stringBuilder = StringBuilder()
    private var s: String? = null
    private var now = 0
    private var n = 0
    private lateinit var l: IntArray
    private lateinit var r: IntArray
    private lateinit var p: IntArray
    private lateinit var c: CharArray

    private fun dfs(x: Int) {
        l[x] = now + 1
        for (v in e[x]!!) {
            dfs(v!!)
        }
        stringBuilder.append(s!![x])
        r[x] = ++now
    }

    private fun matcher() {
        c[0] = '~'
        c[1] = '#'
        for (i in 1..n) {
            c[2 * i + 1] = '#'
            c[2 * i] = stringBuilder[i - 1]
        }
        var j = 1
        var mid = 0
        var localR = 0
        while (j <= 2 * n + 1) {
            if (j <= localR) {
                p[j] = min(p[(mid shl 1) - j], (localR - j + 1))
            }
            while (c[j - p[j]] == c[j + p[j]]) {
                ++p[j]
            }
            if (p[j] + j > localR) {
                localR = p[j] + j - 1
                mid = j
            }
            ++j
        }
    }

    fun findAnswer(parent: IntArray, s: String): BooleanArray {
        n = parent.size
        this.s = s
        for (i in 0 until n) {
            e.add(ArrayList<Int?>())
        }
        for (i in 1 until n) {
            e[parent[i]]!!.add(i)
        }
        l = IntArray(n)
        r = IntArray(n)
        dfs(0)
        c = CharArray(2 * n + 10)
        p = IntArray(2 * n + 10)
        matcher()
        val ans = BooleanArray(n)
        for (i in 0 until n) {
            val mid = (2 * r[i] - 2 * l[i] + 1) / 2 + 2 * l[i]
            ans[i] = p[mid] - 1 >= r[i] - l[i] + 1
        }
        return ans
    }
}
```