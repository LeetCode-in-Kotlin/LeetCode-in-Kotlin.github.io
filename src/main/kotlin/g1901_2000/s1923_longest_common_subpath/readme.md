[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1923\. Longest Common Subpath

Hard

There is a country of `n` cities numbered from `0` to `n - 1`. In this country, there is a road connecting **every pair** of cities.

There are `m` friends numbered from `0` to `m - 1` who are traveling through the country. Each one of them will take a path consisting of some cities. Each path is represented by an integer array that contains the visited cities in order. The path may contain a city **more than once**, but the same city will not be listed consecutively.

Given an integer `n` and a 2D integer array `paths` where `paths[i]` is an integer array representing the path of the <code>i<sup>th</sup></code> friend, return _the length of the **longest common subpath** that is shared by **every** friend's path, or_ `0` _if there is no common subpath at all_.

A **subpath** of a path is a contiguous sequence of cities within that path.

**Example 1:**

**Input:** n = 5, paths = \[\[0,1,2,3,4], 
                           [2,3,4], 
                           [4,0,1,2,3]]

**Output:** 2

**Explanation:** The longest common subpath is [2,3].

**Example 2:**

**Input:** n = 3, paths = \[\[0],[1],[2]]

**Output:** 0

**Explanation:** There is no common subpath shared by the three paths.

**Example 3:**

**Input:** n = 5, paths = \[\[0,1,2,3,4], 
                           [4,3,2,1,0]]

**Output:** 1

**Explanation:** The possible longest common subpaths are [0], [1], [2], [3], and [4]. All have a length of 1.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `m == paths.length`
*   <code>2 <= m <= 10<sup>5</sup></code>
*   <code>sum(paths[i].length) <= 10<sup>5</sup></code>
*   `0 <= paths[i][j] < n`
*   The same city is not listed multiple times consecutively in `paths[i]`.

## Solution

```kotlin
@Suppress("UNUSED_PARAMETER")
class Solution {
    private lateinit var pow: LongArray

    fun longestCommonSubpath(n: Int, paths: Array<IntArray>): Int {
        var res = 0
        var min = Int.MAX_VALUE
        for (path in paths) {
            min = Math.min(min, path.size)
        }
        pow = LongArray(min + 1)
        pow[0]++
        for (i in 1..min) {
            pow[i] = pow[i - 1] * BASE % MOD
        }
        var st = 1
        var end = min
        var mid = (st + end) / 2
        while (st <= end) {
            if (commonSubstring(paths, mid)) {
                res = mid
                st = mid + 1
            } else {
                end = mid - 1
            }
            mid = (st + end) / 2
        }
        return res
    }

    private fun commonSubstring(paths: Array<IntArray>, l: Int): Boolean {
        val set = rollingHash(paths[0], l)
        var i = 1
        val n = paths.size
        while (i < n) {
            set.retainAll(rollingHash(paths[i], l))
            if (set.isEmpty()) {
                return false
            }
            i++
        }
        return true
    }

    private fun rollingHash(a: IntArray, l: Int): HashSet<Long> {
        val set = HashSet<Long>()
        var hash: Long = 0
        for (i in 0 until l) {
            hash = (hash * BASE + a[i]) % MOD
        }
        set.add(hash)
        val n = a.size
        var curr = l
        var prev = 0
        while (curr < n) {
            hash = (hash * BASE % MOD - a[prev] * pow[l] % MOD + a[curr] + MOD) % MOD
            set.add(hash)
            prev++
            curr++
        }
        return set
    }

    companion object {
        private const val BASE: Long = 100001
        private val MOD = (Math.pow(10.0, 11.0) + 7).toLong()
    }
}
```