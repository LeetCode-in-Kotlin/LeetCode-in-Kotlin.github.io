[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2657\. Find the Prefix Common Array of Two Arrays

Medium

You are given two **0-indexed** integer permutations `A` and `B` of length `n`.

A **prefix common array** of `A` and `B` is an array `C` such that `C[i]` is equal to the count of numbers that are present at or before the index `i` in both `A` and `B`.

Return _the **prefix common array** of_ `A` _and_ `B`.

A sequence of `n` integers is called a **permutation** if it contains all integers from `1` to `n` exactly once.

**Example 1:**

**Input:** A = [1,3,2,4], B = [3,1,2,4]

**Output:** [0,2,3,4]

**Explanation:** At i = 0: no number is common, so C[0] = 0. 

At i = 1: 1 and 3 are common in A and B, so C[1] = 2. 

At i = 2: 1, 2, and 3 are common in A and B, so C[2] = 3. 

At i = 3: 1, 2, 3, and 4 are common in A and B, so C[3] = 4.

**Example 2:**

**Input:** A = [2,3,1], B = [3,1,2]

**Output:** [0,1,3]

**Explanation:** At i = 0: no number is common, so C[0] = 0. 

At i = 1: only 3 is common in A and B, so C[1] = 1. 

At i = 2: 1, 2, and 3 are common in A and B, so C[2] = 3.

**Constraints:**

*   `1 <= A.length == B.length == n <= 50`
*   `1 <= A[i], B[i] <= n`
*   `It is guaranteed that A and B are both a permutation of n integers.`

## Solution

```kotlin
class Solution {
    fun findThePrefixCommonArray(a: IntArray, b: IntArray): IntArray {
        val hsA = HashSet<Int>()
        val hsB = HashSet<Int>()
        val addedA = HashSet<Int>()
        val addedB = HashSet<Int>()
        val res = IntArray(a.size)
        for (i in a.indices) {
            val numA = a[i]
            val numB = b[i]
            hsA.add(numA)
            hsB.add(numB)
            if (i > 0) res[i] += res[i - 1] else res[i] = 0
            if (numA in hsB && numA !in addedB) {
                addedA.add(numA)
                res[i] += 1
            }
            if (numB in hsA && numB !in addedA) {
                addedB.add(numB)
                res[i] += 1
            }
        }
        return res
    }
}
```