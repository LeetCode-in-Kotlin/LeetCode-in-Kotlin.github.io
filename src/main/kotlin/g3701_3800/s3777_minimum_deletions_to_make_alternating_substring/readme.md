[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3777\. Minimum Deletions to Make Alternating Substring

Hard

You are given a string `s` of length `n` consisting only of the characters `'A'` and `'B'`.

You are also given a 2D integer array `queries` of length `q`, where each `queries[i]` is one of the following:

*   `[1, j]`: **Flip** the character at index `j` of `s` i.e. `'A'` changes to `'B'` (and vice versa). This operation mutates `s` and affects subsequent queries.
*   `[2, l, r]`: **Compute** the **minimum** number of character deletions required to make the **substring** `s[l..r]` **alternating**. This operation does not modify `s`; the length of `s` remains `n`.

A **substring** is **alternating** if no two **adjacent** characters are **equal**. A substring of length 1 is always alternating.

Return an integer array `answer`, where `answer[i]` is the result of the <code>i<sup>th</sup></code> query of type `[2, l, r]`.

**Example 1:**

**Input:** s = "ABA", queries = \[\[2,1,2],[1,1],[2,0,2]]

**Output:** [0,2]

**Explanation:**

| i | queries[i] | j | l | r | s before query | s[l..r] | Result | Answer |
|---|------------|---|---|---|----------------|---------|--------|--------|
| 0 | [2, 1, 2]  | - | 1 | 2 | `"ABA"`        | `"BA"`  | Already alternating | 0 |
| 1 | [1, 1]     | 1 | - | - | `"ABA"`        | -       | Flip `s[1]` from `'B'` to `'A'` | - |
| 2 | [2, 0, 2]  | - | 0 | 2 | `"AAA"`        | `"AAA"` | Delete any two `'A'`s to get `"A"` | 2 |

Thus, the answer is `[0, 2]`.

**Example 2:**

**Input:** s = "ABB", queries = \[\[2,0,2],[1,2],[2,0,2]]

**Output:** [1,0]

**Explanation:**

| i | queries[i] | j | l | r | s before query | s[l..r] | Result | Answer |
|---|------------|---|---|---|----------------|---------|--------|--------|
| 0 | [2, 0, 2]  | - | 0 | 2 | `"ABB"`        | `"ABB"` | Delete one `'B'` to get `"AB"` | 1 |
| 1 | [1, 2]     | 2 | - | - | `"ABB"`        | -       | Flip `s[2]` from `'B'` to `'A'` | - |
| 2 | [2, 0, 2]  | - | 0 | 2 | `"ABA"`        | `"ABA"` | Already alternating | 0 |

Thus, the answer is `[1, 0]`.

**Example 3:**

**Input:** s = "BABA", queries = \[\[2,0,3],[1,1],[2,1,3]]

**Output:** [0,1]

**Explanation:**

| i | queries[i] | j | l | r | s before query | s[l..r] | Result | Answer |
|---|------------|---|---|---|----------------|---------|--------|--------|
| 0 | [2, 0, 3]  | - | 0 | 3 | `"BABA"`       | `"BABA"`| Already alternating | 0 |
| 1 | [1, 1]     | 1 | - | - | `"BABA"`       | -       | Flip `s[1]` from `'A'` to `'B'` | - |
| 2 | [2, 1, 3]  | - | 1 | 3 | `"BBBA"`       | `"BBA"` | Delete one `'B'` to get `"BA"` | 1 |

Thus, the answer is `[0, 1]`.

**Constraints:**

*   <code>1 <= n == s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'A'` or `'B'`.
*   <code>1 <= q == queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2` or `3`
    *   `queries[i] == [1, j]` or,
    *   `queries[i] == [2, l, r]`
    *   `0 <= j <= n - 1`
    *   `0 <= l <= r <= n - 1`

## Solution

```kotlin
class Solution {
    private lateinit var f: IntArray
    private var n = 0

    private fun add(i: Int, v: Int) {
        var i = i
        i++
        while (i <= n) {
            f[i] += v
            i += i and -i
        }
    }

    private fun sum(i: Int): Int {
        var i = i
        var s = 0
        i++
        while (i > 0) {
            s += f[i]
            i -= i and -i
        }
        return s
    }

    fun minDeletions(s: String, q: Array<IntArray>): IntArray? {
        val a = s.toCharArray()
        n = a.size
        f = IntArray(n + 1)
        var i = 0
        while (i + 1 < n) {
            if (a[i] == a[i + 1]) {
                add(i, 1)
            }
            i++
        }
        val ans = IntArray(q.size)
        var k = 0
        for (e in q) {
            if (e[0] == 1) {
                val j = e[1]
                if (j > 0 && a[j] == a[j - 1]) {
                    add(j - 1, -1)
                }
                if (j + 1 < n && a[j] == a[j + 1]) {
                    add(j, -1)
                }
                a[j] = if (a[j] == 'A') 'B' else 'A'
                if (j > 0 && a[j] == a[j - 1]) {
                    add(j - 1, 1)
                }
                if (j + 1 < n && a[j] == a[j + 1]) {
                    add(j, 1)
                }
            } else {
                val l = e[1]
                val r = e[2]
                ans[k++] = if (l < r) sum(r - 1) - sum(l - 1) else 0
            }
        }
        return ans.copyOf(k)
    }
}
```