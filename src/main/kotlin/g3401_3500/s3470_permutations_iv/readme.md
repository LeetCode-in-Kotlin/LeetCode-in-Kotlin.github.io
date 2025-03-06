[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3470\. Permutations IV

Hard

Given two integers, `n` and `k`, an **alternating permutation** is a permutation of the first `n` positive integers such that no **two** adjacent elements are both odd or both even.

Return the **k-th** **alternating permutation** sorted in _lexicographical order_. If there are fewer than `k` valid **alternating permutations**, return an empty list.

**Example 1:**

**Input:** n = 4, k = 6

**Output:** [3,4,1,2]

**Explanation:**

The lexicographically-sorted alternating permutations of `[1, 2, 3, 4]` are:

1.  `[1, 2, 3, 4]`
2.  `[1, 4, 3, 2]`
3.  `[2, 1, 4, 3]`
4.  `[2, 3, 4, 1]`
5.  `[3, 2, 1, 4]`
6.  `[3, 4, 1, 2]` ← 6th permutation
7.  `[4, 1, 2, 3]`
8.  `[4, 3, 2, 1]`

Since `k = 6`, we return `[3, 4, 1, 2]`.

**Example 2:**

**Input:** n = 3, k = 2

**Output:** [3,2,1]

**Explanation:**

The lexicographically-sorted alternating permutations of `[1, 2, 3]` are:

1.  `[1, 2, 3]`
2.  `[3, 2, 1]` ← 2nd permutation

Since `k = 2`, we return `[3, 2, 1]`.

**Example 3:**

**Input:** n = 2, k = 3

**Output:** []

**Explanation:**

The lexicographically-sorted alternating permutations of `[1, 2]` are:

1.  `[1, 2]`
2.  `[2, 1]`

There are only 2 alternating permutations, but `k = 3`, which is out of range. Thus, we return an empty list `[]`.

**Constraints:**

*   `1 <= n <= 100`
*   <code>1 <= k <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    private val maxFac = 100_000_000L

    fun permute(n: Int, k: Long): IntArray {
        var res = IntArray(n)
        var k = k - 1
        val fac = LongArray(n / 2 + 1)
        fac[0] = 1
        for (i in 1..n / 2) {
            fac[i] = fac[i - 1] * i
            if (fac[i] >= maxFac) {
                fac[i] = maxFac
            }
        }
        var evenNum = n / 2
        var oddNum = n - evenNum
        var evens = mutableListOf<Int>()
        var odds = mutableListOf<Int>()
        for (i in 1..n) {
            if (i % 2 == 0) {
                evens.add(i)
            } else {
                odds.add(i)
            }
        }
        for (i in 0..<n) {
            if (i == 0) {
                if (n % 2 == 0) {
                    val trailCombs = fac[evenNum] * fac[evenNum - 1]
                    val leadIdx = (k / trailCombs).toInt()
                    if (leadIdx + 1 > n) return IntArray(0)
                    res[i] = leadIdx + 1
                    if ((leadIdx + 1) % 2 == 0) {
                        evens.remove(leadIdx + 1)
                    } else {
                        odds.remove(leadIdx + 1)
                    }
                    k = k % trailCombs
                } else {
                    val trailCombs = fac[oddNum - 1] * fac[evenNum]
                    val leadIdx = (k / trailCombs).toInt()
                    if (leadIdx >= odds.size) return IntArray(0)
                    val num = odds.removeAt(leadIdx)
                    res[i] = num
                    k = k % trailCombs
                }
            } else {
                if (res[i - 1] % 2 == 0) {
                    val trailCombs = fac[evenNum] * fac[oddNum - 1]
                    val leadIdx = (k / trailCombs).toInt()
                    val num = odds.removeAt(leadIdx)
                    res[i] = num
                    k = k % trailCombs
                } else {
                    val trailCombs = fac[evenNum - 1] * fac[oddNum ]
                    val leadIdx = (k / trailCombs).toInt()
                    val num = evens.removeAt(leadIdx)
                    res[i] = num
                    k = k % trailCombs
                }
            }
            if (res[i] % 2 == 0) {
                evenNum--
            } else {
                oddNum--
            }
        }
        return res
    }
}
```