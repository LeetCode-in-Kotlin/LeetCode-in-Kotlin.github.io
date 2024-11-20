[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2975\. Maximum Square Area by Removing Fences From a Field

Medium

There is a large `(m - 1) x (n - 1)` rectangular field with corners at `(1, 1)` and `(m, n)` containing some horizontal and vertical fences given in arrays `hFences` and `vFences` respectively.

Horizontal fences are from the coordinates `(hFences[i], 1)` to `(hFences[i], n)` and vertical fences are from the coordinates `(1, vFences[i])` to `(m, vFences[i])`.

Return _the **maximum** area of a **square** field that can be formed by **removing** some fences (**possibly none**) or_ `-1` _if it is impossible to make a square field_.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note:** The field is surrounded by two horizontal fences from the coordinates `(1, 1)` to `(1, n)` and `(m, 1)` to `(m, n)` and two vertical fences from the coordinates `(1, 1)` to `(m, 1)` and `(1, n)` to `(m, n)`. These fences **cannot** be removed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/11/05/screenshot-from-2023-11-05-22-40-25.png)

**Input:** m = 4, n = 3, hFences = [2,3], vFences = [2]

**Output:** 4

**Explanation:** Removing the horizontal fence at 2 and the vertical fence at 2 will give a square field of area 4.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/11/22/maxsquareareaexample1.png)

**Input:** m = 6, n = 7, hFences = [2], vFences = [4]

**Output:** -1

**Explanation:** It can be proved that there is no way to create a square field by removing fences.

**Constraints:**

*   <code>3 <= m, n <= 10<sup>9</sup></code>
*   `1 <= hFences.length, vFences.length <= 600`
*   `1 < hFences[i] < m`
*   `1 < vFences[i] < n`
*   `hFences` and `vFences` are unique.

## Solution

```kotlin
class Solution {
    fun maximizeSquareArea(
        m: Int,
        n: Int,
        hFences: IntArray,
        vFences: IntArray,
    ): Int {
        val hFencesWithBorder = IntArray(hFences.size + 2)
        System.arraycopy(hFences, 0, hFencesWithBorder, 0, hFences.size)
        hFencesWithBorder[hFences.size] = 1
        hFencesWithBorder[hFences.size + 1] = m
        hFencesWithBorder.sort()
        val edgeSet: MutableSet<Int> = HashSet()
        run {
            var i = 0
            while (i < hFencesWithBorder.size) {
                var j = i + 1
                while (j < hFencesWithBorder.size) {
                    edgeSet.add(hFencesWithBorder[j] - hFencesWithBorder[i])
                    j += 1
                }
                i += 1
            }
        }
        var maxEdge = -1
        val vFencesWithBorder = IntArray(vFences.size + 2)
        System.arraycopy(vFences, 0, vFencesWithBorder, 0, vFences.size)
        vFencesWithBorder[vFences.size] = 1
        vFencesWithBorder[vFences.size + 1] = n
        vFencesWithBorder.sort()
        var i = 0
        while (i < vFencesWithBorder.size) {
            var j = i + 1
            while (j < vFencesWithBorder.size) {
                val curEdge = vFencesWithBorder[j] - vFencesWithBorder[i]
                if (edgeSet.contains(curEdge) && curEdge > maxEdge) {
                    maxEdge = curEdge
                }
                j += 1
            }
            i += 1
        }
        val mod = 1e9.toInt() + 7
        return if (maxEdge != -1) (maxEdge.toLong() * maxEdge % mod).toInt() else -1
    }
}
```