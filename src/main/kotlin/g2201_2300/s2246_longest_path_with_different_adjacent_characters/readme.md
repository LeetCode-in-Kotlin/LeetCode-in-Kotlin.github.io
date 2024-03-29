[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2246\. Longest Path With Different Adjacent Characters

Hard

You are given a **tree** (i.e. a connected, undirected graph that has no cycles) **rooted** at node `0` consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by a **0-indexed** array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node `0` is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to node `i`.

Return _the length of the **longest path** in the tree such that no pair of **adjacent** nodes on the path have the same character assigned to them._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png)

**Input:** parent = [-1,0,0,1,1,2], s = "abacbe"

**Output:** 3

**Explanation:** The longest path where each two adjacent nodes have different characters in the tree is the path: 0 -> 1 -> 3. The length of this path is 3, so 3 is returned.

It can be proven that there is no longer path that satisfies the conditions. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/25/graph2drawio.png)

**Input:** parent = [-1,0,0,0], s = "aabc"

**Output:** 3

**Explanation:** The longest path where each two adjacent nodes have different characters is the path: 2 -> 0 -> 3. The length of this path is 3, so 3 is returned. 

**Constraints:**

*   `n == parent.length == s.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= parent[i] <= n - 1` for all `i >= 1`
*   `parent[0] == -1`
*   `parent` represents a valid tree.
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
import java.util.LinkedList

class Solution {
    fun longestPath(parent: IntArray, s: String): Int {
        // for first max length
        val first = IntArray(s.length)
        first.fill(0)
        // for second max length
        val second = IntArray(s.length)
        second.fill(0)
        // for number of children for this node
        val children = IntArray(s.length)
        children.fill(0)
        for (i in 1 until s.length) {
            // calculate all children for each node
            children[parent[i]]++
        }
        // for traversal from leafs to root
        val st = LinkedList<Int>()
        // put all leafs
        for (i in 1 until s.length) {
            if (children[i] == 0) {
                st.add(i)
            }
        }
        // traversal from leafs to root
        while (st.isNotEmpty()) {
            // fetch current node
            val i = st.pollLast()
            // if we in root - ignore it
            if (i == 0) {
                continue
            }
            if (--children[parent[i]] == 0) {
                // decrease number of children by parent node and if number of children
                st.add(parent[i])
            }
            // is equal 0 - our parent became a leaf
            // if letters isn't equal
            if (s[parent[i]] != s[i]) {
                // fetch maximal path from node
                val maxi = 1 + Math.max(first[i], second[i])
                // and update maximal first and second path from parent
                if (maxi >= first[parent[i]]) {
                    second[parent[i]] = first[parent[i]]
                    first[parent[i]] = maxi
                } else if (second[parent[i]] < maxi) {
                    second[parent[i]] = maxi
                }
            }
        }
        // fetch answer
        var ans = 0
        for (i in 0 until s.length) {
            ans = Math.max(ans, first[i] + second[i])
        }
        return ans + 1
    }
}
```