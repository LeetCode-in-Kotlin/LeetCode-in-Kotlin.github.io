[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 923\. 3Sum With Multiplicity

Medium

Given an integer array `arr`, and an integer `target`, return the number of tuples `i, j, k` such that `i < j < k` and `arr[i] + arr[j] + arr[k] == target`.

As the answer can be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [1,1,2,2,3,3,4,4,5,5], target = 8

**Output:** 20

**Explanation:**

    Enumerating by the values (arr[i], arr[j], arr[k]): 
    (1, 2, 5) occurs 8 times; 
    (1, 3, 4) occurs 8 times; 
    (2, 2, 4) occurs 2 times; 
    (2, 3, 3) occurs 2 times.

**Example 2:**

**Input:** arr = [1,1,2,2,2,2], target = 5

**Output:** 12

**Explanation:**

arr[i] = 1, arr[j] = arr[k] = 2 occurs 12 times:

We choose one 1 from [1,1] in 2 ways,

and two 2s from [2,2,2,2] in 6 ways.

**Constraints:**

*   `3 <= arr.length <= 3000`
*   `0 <= arr[i] <= 100`
*   `0 <= target <= 300`

## Solution

```kotlin
class Solution {
    fun threeSumMulti(arr: IntArray, target: Int): Int {
        var answer = 0
        val countRight = IntArray(MAX + 1)
        for (num in arr) {
            ++countRight[num]
        }
        val countLeft = IntArray(MAX + 1)
        for (j in 0 until arr.size - 1) {
            --countRight[arr[j]]
            val remains = target - arr[j]
            if (remains <= 2 * MAX) {
                for (v in 0..remains.coerceAtMost(MAX)) {
                    if (remains - v <= MAX) {
                        val count = countRight[v] * countLeft[remains - v]
                        if (count > 0) {
                            answer = (answer + count) % MOD
                        }
                    }
                }
            }
            ++countLeft[arr[j]]
        }
        return answer
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
        private const val MAX = 100
    }
}
```