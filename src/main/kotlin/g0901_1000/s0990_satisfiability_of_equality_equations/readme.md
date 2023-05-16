[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 990\. Satisfiability of Equality Equations

Medium

You are given an array of strings `equations` that represent relationships between variables where each string `equations[i]` is of length `4` and takes one of two different forms: <code>"x<sub>i</sub>==y<sub>i</sub>"</code> or <code>"x<sub>i</sub>!=y<sub>i</sub>"</code>.Here, <code>x<sub>i</sub></code> and <code>y<sub>i</sub></code> are lowercase letters (not necessarily different) that represent one-letter variable names.

Return `true` _if it is possible to assign integers to variable names so as to satisfy all the given equations, or_ `false` _otherwise_.

**Example 1:**

**Input:** equations = ["a==b","b!=a"]

**Output:** false

**Explanation:** If we assign say, a = 1 and b = 1, then the first equation is satisfied, but not the second. There is no way to assign the variables to satisfy both equations.

**Example 2:**

**Input:** equations = ["b==a","a==b"]

**Output:** true

**Explanation:** We could assign a = 1 and b = 1 to satisfy both equations.

**Constraints:**

*   `1 <= equations.length <= 500`
*   `equations[i].length == 4`
*   `equations[i][0]` is a lowercase letter.
*   `equations[i][1]` is either `'='` or `'!'`.
*   `equations[i][2]` is `'='`.
*   `equations[i][3]` is a lowercase letter.

## Solution

```kotlin
class Solution {
    private lateinit var par: IntArray
    fun equationsPossible(equations: Array<String>): Boolean {
        var counter = 0
        val map: HashMap<Char, Int> = HashMap()
        for (str in equations) {
            var ch = str[0]
            if (!map.containsKey(ch)) {
                map[ch] = counter
                counter++
            }
            ch = str[3]
            if (!map.containsKey(ch)) {
                map[ch] = counter
                counter++
            }
        }
        par = IntArray(counter)
        for (i in par.indices) {
            par[i] = i
        }
        for (str in equations) {
            val oper = str.substring(1, 3)
            if (oper == "==") {
                val px = find(map[str[0]])
                val py = find(map[str[3]])
                if (px != py) {
                    par[px] = py
                }
            }
        }
        for (str in equations) {
            val oper = str.substring(1, 3)
            if (oper == "!=") {
                val px = find(map[str[0]])
                val py = find(map[str[3]])
                if (px == py) {
                    return false
                }
            }
        }
        return true
    }

    private fun find(x: Int?): Int {
        if (par[x!!] == x) {
            return x
        }
        par[x] = find(par[x])
        return par[x]
    }
}
```