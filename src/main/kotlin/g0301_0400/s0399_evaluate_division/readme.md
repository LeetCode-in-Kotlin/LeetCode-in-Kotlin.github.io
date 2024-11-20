[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 399\. Evaluate Division

Medium

You are given an array of variable pairs `equations` and an array of real numbers `values`, where <code>equations[i] = [A<sub>i</sub>, B<sub>i</sub>]</code> and `values[i]` represent the equation <code>A<sub>i</sub> / B<sub>i</sub> = values[i]</code>. Each <code>A<sub>i</sub></code> or <code>B<sub>i</sub></code> is a string that represents a single variable.

You are also given some `queries`, where <code>queries[j] = [C<sub>j</sub>, D<sub>j</sub>]</code> represents the <code>j<sup>th</sup></code> query where you must find the answer for <code>C<sub>j</sub> / D<sub>j</sub> = ?</code>.

Return _the answers to all queries_. If a single answer cannot be determined, return `-1.0`.

**Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Example 1:**

**Input:** equations = \[\["a","b"],["b","c"]], values = [2.0,3.0], queries = \[\["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]

**Output:** [6.00000,0.50000,-1.00000,1.00000,-1.00000]

**Explanation:** Given: _a / b = 2.0_, _b / c = 3.0_ queries are: _a / c = ?_, _b / a = ?_, _a / e = ?_, _a / a = ?_, _x / x = ?_ return: [6.0, 0.5, -1.0, 1.0, -1.0 ]

**Example 2:**

**Input:** equations = \[\["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = \[\["a","c"],["c","b"],["bc","cd"],["cd","bc"]]

**Output:** [3.75000,0.40000,5.00000,0.20000]

**Example 3:**

**Input:** equations = \[\["a","b"]], values = [0.5], queries = \[\["a","b"],["b","a"],["a","c"],["x","y"]]

**Output:** [0.50000,2.00000,-1.00000,-1.00000]

**Constraints:**

*   `1 <= equations.length <= 20`
*   `equations[i].length == 2`
*   <code>1 <= A<sub>i</sub>.length, B<sub>i</sub>.length <= 5</code>
*   `values.length == equations.length`
*   `0.0 < values[i] <= 20.0`
*   `1 <= queries.length <= 20`
*   `queries[i].length == 2`
*   <code>1 <= C<sub>j</sub>.length, D<sub>j</sub>.length <= 5</code>
*   <code>A<sub>i</sub>, B<sub>i</sub>, C<sub>j</sub>, D<sub>j</sub></code> consist of lower case English letters and digits.

## Solution

```kotlin
@Suppress("kotlin:S6518")
class Solution {
    private var root: MutableMap<String?, String?>? = null
    private var rate: MutableMap<String?, Double>? = null

    fun calcEquation(
        equations: List<List<String?>>,
        values: DoubleArray,
        queries: List<List<String?>>,
    ): DoubleArray {
        root = HashMap()
        rate = HashMap()
        val n = equations.size
        for (equation in equations) {
            val x = equation[0]
            val y = equation[1]
            (root as HashMap<String?, String?>)[x] = x
            (root as HashMap<String?, String?>)[y] = y
            (rate as HashMap<String?, Double>)[x] = 1.0
            (rate as HashMap<String?, Double>)[y] = 1.0
        }
        for (i in 0 until n) {
            val x = equations[i][0]
            val y = equations[i][1]
            union(x, y, values[i])
        }
        val result = DoubleArray(queries.size)
        for (i in queries.indices) {
            val x = queries[i][0]
            val y = queries[i][1]
            if (!(root as HashMap<String?, String?>).containsKey(x) ||
                !(root as HashMap<String?, String?>).containsKey(y)
            ) {
                result[i] = -1.0
                continue
            }
            val rootX = findRoot(x, x, 1.0)
            val rootY = findRoot(y, y, 1.0)
            result[i] = if (rootX == rootY) {
                (rate as HashMap<String?, Double>).get(x)!! /
                    (rate as HashMap<String?, Double>).get(y)!!
            } else {
                -1.0
            }
        }
        return result
    }

    private fun union(x: String?, y: String?, v: Double) {
        val rootX = findRoot(x, x, 1.0)
        val rootY = findRoot(y, y, 1.0)
        root!![rootX] = rootY
        val r1 = rate!![x]!!
        val r2 = rate!![y]!!
        rate!![rootX] = v * r2 / r1
    }

    private fun findRoot(originalX: String?, x: String?, r: Double): String? {
        if (root!![x] == x) {
            root!![originalX] = x
            rate!![originalX] = r * rate!![x]!!
            return x
        }
        return findRoot(originalX, root!![x], r * rate!![x]!!)
    }
}
```