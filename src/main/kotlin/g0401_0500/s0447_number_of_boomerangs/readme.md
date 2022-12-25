[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 447\. Number of Boomerangs

Medium

You are given `n` `points` in the plane that are all **distinct**, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>. A **boomerang** is a tuple of points `(i, j, k)` such that the distance between `i` and `j` equals the distance between `i` and `k` **(the order of the tuple matters)**.

Return _the number of boomerangs_.

**Example 1:**

**Input:** points = \[\[0,0],[1,0],[2,0]]

**Output:** 2

**Explanation:** The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]].

**Example 2:**

**Input:** points = \[\[1,1],[2,2],[3,3]]

**Output:** 2

**Example 3:**

**Input:** points = \[\[1,1]]

**Output:** 0

**Constraints:**

*   `n == points.length`
*   `1 <= n <= 500`
*   `points[i].length == 2`
*   <code>-10<sup>4</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>4</sup></code>
*   All the points are **unique**.

## Solution

```kotlin
class Solution {
    fun numberOfBoomerangs(points: Array<IntArray>): Int {
        val m: HashMap<Int, Int> = HashMap()
        var ans = 0
        for (i in points.indices) {
            for (j in points.indices) {
                if (i == j) {
                    continue
                }
                val dis = dist(points[i], points[j])
                val prev = m.getOrDefault(dis, 0)
                if (prev >= 1) {
                    ans += prev * 2
                }
                m[dis] = prev + 1
            }
            m.clear()
        }
        return ans
    }

    private fun dist(d1: IntArray, d2: IntArray): Int {
        return (d1[0] - d2[0]) * (d1[0] - d2[0]) + (d1[1] - d2[1]) * (d1[1] - d2[1])
    }
}
```