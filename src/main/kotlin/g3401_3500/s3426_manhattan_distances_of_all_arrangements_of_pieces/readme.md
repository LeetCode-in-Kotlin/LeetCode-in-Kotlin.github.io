[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3426\. Manhattan Distances of All Arrangements of Pieces

Hard

You are given three integers `m`, `n`, and `k`.

There is a rectangular grid of size `m × n` containing `k` identical pieces. Return the sum of Manhattan distances between every pair of pieces over all **valid arrangements** of pieces.

A **valid arrangement** is a placement of all `k` pieces on the grid with **at most** one piece per cell.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

The Manhattan Distance between two cells <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and <code>(x<sub>j</sub>, y<sub>j</sub>)</code> is <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.

**Example 1:**

**Input:** m = 2, n = 2, k = 2

**Output:** 8

**Explanation:**

The valid arrangements of pieces on the board are:

![](https://assets.leetcode.com/uploads/2024/12/25/4040example1.drawio)![](https://assets.leetcode.com/uploads/2024/12/25/untitled-diagramdrawio.png)

*   In the first 4 arrangements, the Manhattan distance between the two pieces is 1.
*   In the last 2 arrangements, the Manhattan distance between the two pieces is 2.

Thus, the total Manhattan distance across all valid arrangements is `1 + 1 + 1 + 1 + 2 + 2 = 8`.

**Example 2:**

**Input:** m = 1, n = 4, k = 3

**Output:** 20

**Explanation:**

The valid arrangements of pieces on the board are:

![](https://assets.leetcode.com/uploads/2024/12/25/4040example2drawio.png)

*   The first and last arrangements have a total Manhattan distance of `1 + 1 + 2 = 4`.
*   The middle two arrangements have a total Manhattan distance of `1 + 2 + 3 = 6`.

The total Manhattan distance between all pairs of pieces across all arrangements is `4 + 6 + 6 + 4 = 20`.

**Constraints:**

*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>2 <= m * n <= 10<sup>5</sup></code>
*   `2 <= k <= m * n`

## Solution

```kotlin
class Solution {
    private fun comb(a: Long, b: Long, mod: Long): Long {
        if (b > a) {
            return 0
        }
        var numer: Long = 1
        var denom: Long = 1
        for (i in 0..<b) {
            numer = numer * (a - i) % mod
            denom = denom * (i + 1) % mod
        }
        var denomInv: Long = 1
        var exp = mod - 2
        while (exp > 0) {
            if (exp % 2 > 0) {
                denomInv = denomInv * denom % mod
            }
            denom = denom * denom % mod
            exp /= 2
        }
        return numer * denomInv % mod
    }

    fun distanceSum(m: Int, n: Int, k: Int): Int {
        var res: Long = 0
        val mod: Long = 1000000007
        val base = comb(m.toLong() * n - 2, k - 2L, mod)
        for (d in 1..<n) {
            res = (res + d.toLong() * (n - d) % mod * m % mod * m % mod) % mod
        }
        for (d in 1..<m) {
            res = (res + d.toLong() * (m - d) % mod * n % mod * n % mod) % mod
        }
        return (res * base % mod).toInt()
    }
}
```