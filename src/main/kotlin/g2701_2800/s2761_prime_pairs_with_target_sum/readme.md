[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2761\. Prime Pairs With Target Sum

Medium

You are given an integer `n`. We say that two integers `x` and `y` form a prime number pair if:

*   `1 <= x <= y <= n`
*   `x + y == n`
*   `x` and `y` are prime numbers

Return _the 2D sorted list of prime number pairs_ <code>[x<sub>i</sub>, y<sub>i</sub>]</code>. The list should be sorted in **increasing** order of <code>x<sub>i</sub></code>. If there are no prime number pairs at all, return _an empty array_.

**Note:** A prime number is a natural number greater than `1` with only two factors, itself and `1`.

**Example 1:**

**Input:** n = 10

**Output:** [[3,7],[5,5]]

**Explanation:**

In this example, there are two prime pairs that satisfy the criteria.

These pairs are [3,7] and [5,5], and we return them in the sorted order as described in the problem statement.

**Example 2:**

**Input:** n = 2

**Output:** []

**Explanation:**

We can show that there is no prime number pair that gives a sum of 2, so we return an empty array.

**Constraints:**

*   <code>1 <= n <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun findPrimePairs(n: Int): List<List<Int>> {
        val answer: MutableList<List<Int>> = ArrayList()
        for (a in list) {
            val other = n - a
            if (other < n / 2 || a > n / 2) break
            if (primes.contains(other)) answer.add(listOf(a, other))
        }
        return answer
    }

    companion object {
        private val primes: HashSet<Int> = HashSet()
        private val list: MutableList<Int> = ArrayList()

        init {
            val m = 1000001
            val visited = BooleanArray(m)
            for (i in 2 until m) {
                if (!visited[i]) {
                    primes.add(i)
                    list.add(i)
                    var j: Int = i
                    while (j < m) {
                        visited[j] = true
                        j += i
                    }
                }
            }
        }
    }
}
```