[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3841\. Palindromic Path Queries in a Tree

Hard

You are given an undirected tree with `n` nodes labeled 0 to `n - 1`. This is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates an undirected edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

You are also given a string `s` of length `n` consisting of lowercase English letters, where `s[i]` represents the character assigned to node `i`.

You are also given a string array `queries`, where each `queries[i]` is either:

*   <code>"update u<sub>i</sub> c"</code>: Change the character at node <code>u<sub>i</sub></code> to `c`. Formally, update <code>s[u<sub>i</sub>] = c</code>.
*   <code>"query u<sub>i</sub> v<sub>i</sub>"</code>: Determine whether the string formed by the characters on the **unique** path from <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code> (inclusive) can be **rearranged** into a **palindrome**.

Return a boolean array `answer`, where `answer[j]` is `true` if the <code>j<sup>th</sup></code> query of type <code>"query u<sub>i</sub> v<sub>i</sub>"</code> can be rearranged into a **palindrome**, and `false` otherwise.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1],[1,2]], s = "aac", queries = ["query 0 2","update 1 b","query 0 2"]

**Output:** [true,false]

**Explanation:**

*   `"query 0 2"`: Path `0 → 1 → 2` gives `"aac"`, which can be rearranged to form `"aca"`, a palindrome. Thus, `answer[0] = true`.
*   `"update 1 b"`: Update node 1 to `'b'`, now `s = "abc"`.
*   `"query 0 2"`: Path characters are `"abc"`, which cannot be rearranged to form a palindrome. Thus, `answer[1] = false`.

Thus, `answer = [true, false]`.

**Example 2:**

**Input:** n = 4, edges = \[\[0,1],[0,2],[0,3]], s = "abca", queries = ["query 1 2","update 0 b","query 2 3","update 3 a","query 1 3"]

**Output:** [false,false,true]

**Explanation:**

*   `"query 1 2"`: Path `1 → 0 → 2` gives `"bac"`, which cannot be rearranged to form a palindrome. Thus, `answer[0] = false`.
*   `"update 0 b"`: Update node 0 to `'b'`, now `s = "bbca"`.
*   `"query 2 3"`: Path `2 → 0 → 3` gives `"cba"`, which cannot be rearranged to form a palindrome. Thus, `answer[1] = false`.
*   `"update 3 a"`: Update node 3 to `'a'`, `s = "bbca"`.
*   `"query 1 3"`: Path `1 → 0 → 3` gives `"bba"`, which can be rearranged to form `"bab"`, a palindrome. Thus, `answer[2] = true`.

Thus, `answer = [false, false, true]`.

**Constraints:**

*   <code>1 <= n == s.length <= 5 * 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   `s` consists of lowercase English letters.
*   The input is generated such that `edges` represents a valid tree.
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
    *   <code>queries[i] = "update u<sub>i</sub> c"</code> or
    *   <code>queries[i] = "query u<sub>i</sub> v<sub>i</sub>"</code>
    *   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
    *   `c` is a lowercase English letter.

## Solution

```kotlin
class Solution {
    private lateinit var head: IntArray
    private lateinit var to: IntArray
    private lateinit var next: IntArray
    private lateinit var lift: Array<IntArray>
    private lateinit var first: IntArray
    private lateinit var last: IntArray
    private lateinit var depth: IntArray
    private var time = 0
    private var maxPower = 0

    fun palindromePath(n: Int, edges: Array<IntArray>, s: String, queries: Array<String>): MutableList<Boolean> {
        this.first = IntArray(n)
        this.last = IntArray(n)
        this.maxPower = 31 - Integer.numberOfLeadingZeros(n)
        this.depth = IntArray(n)
        this.lift = Array<IntArray>(maxPower + 1) { IntArray(n) }
        val m = edges.size
        this.head = IntArray(n)
        this.to = IntArray(m shl 1)
        this.next = IntArray(m shl 1)
        head.fill(-1)
        for (i in 0..<m) {
            val a = edges[i][0]
            val b = edges[i][1]
            to[i shl 1] = b
            next[i shl 1] = head[a]
            head[a] = i shl 1
            to[i shl 1 or 1] = a
            next[i shl 1 or 1] = head[b]
            head[b] = i shl 1 or 1
        }
        dfs(0, -1)
        val bit = BIT(n + 1)
        val arr = s.toCharArray()
        for (i in 0..<n) {
            bit.update(first[i], 1 shl arr[i].code - 'a'.code)
            bit.update(last[i] + 1, 1 shl arr[i].code - 'a'.code)
        }
        val list = ArrayList<Boolean>(queries.size)
        for (query in queries) {
            if (query[0] == 'u') {
                val i = query.substring(7, query.length - 2).toInt()
                val c = query[query.length - 1]
                if (c == arr[i]) {
                    continue
                }
                val filter = 1 shl arr[i].code - 'a'.code or (1 shl c.code - 'a'.code)
                bit.update(first[i], filter)
                arr[i] = c
                bit.update(last[i] + 1, filter)
            } else {
                val i1 = query.indexOf(' ')
                val i2 = query.lastIndexOf(' ')
                val a = query.substring(i1 + 1, i2).toInt()
                val b = query.substring(i2 + 1).toInt()
                val r = bit.query(first[a]) xor bit.query(first[b]) xor (1 shl arr[lca(a, b)].code - 'a'.code)
                list.add((r and r - 1) == 0)
            }
        }
        return list
    }

    private fun lca(a: Int, b: Int): Int {
        var a = a
        var b = b
        if (depth[a] > depth[b]) {
            val temp = a
            a = b
            b = temp
        }
        if (first[a] <= first[b] && last[a] >= last[b]) {
            return a
        }
        var diff = depth[b] - depth[a]
        run {
            var i = maxPower
            while (diff != 0) {
                if (diff >= 1 shl i) {
                    b = lift[i][b]
                    diff -= 1 shl i
                }
                --i
            }
        }
        if (a == b) {
            return a
        }
        for (i in maxPower downTo 0) {
            if (lift[i][a] != lift[i][b]) {
                a = lift[i][a]
                b = lift[i][b]
            }
        }
        return lift[0][a]
    }

    private fun dfs(index: Int, prev: Int) {
        first[index] = ++time
        var x = head[index]
        while (x != -1) {
            val nextIndex = to[x]
            if (nextIndex == prev) {
                x = next[x]
                continue
            }
            depth[nextIndex] = depth[index] + 1
            lift[0][nextIndex] = index
            var i = 1
            while ((1 shl i) < depth[nextIndex]) {
                lift[i][nextIndex] = lift[i - 1][lift[i - 1][nextIndex]]
                i++
            }
            dfs(nextIndex, index)
            x = next[x]
        }
        last[index] = time
    }

    private class BIT(private val n: Int) {
        private val ints: IntArray = IntArray(n + 1)

        fun update(index: Int, `val`: Int) {
            var i = index + 1
            while (i <= n) {
                ints[i] = ints[i] xor `val`
                i += i and -i
            }
        }

        fun query(index: Int): Int {
            var ans = 0
            var i = index + 1
            while (i > 0) {
                ans = ans xor ints[i]
                i -= i and -i
            }
            return ans
        }
    }
}
```