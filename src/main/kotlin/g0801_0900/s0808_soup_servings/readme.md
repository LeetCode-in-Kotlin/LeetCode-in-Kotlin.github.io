[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 808\. Soup Servings

Medium

There are two types of soup: **type A** and **type B**. Initially, we have `n` ml of each type of soup. There are four kinds of operations:

1.  Serve `100` ml of **soup A** and `0` ml of **soup B**,
2.  Serve `75` ml of **soup A** and `25` ml of **soup B**,
3.  Serve `50` ml of **soup A** and `50` ml of **soup B**, and
4.  Serve `25` ml of **soup A** and `75` ml of **soup B**.

When we serve some soup, we give it to someone, and we no longer have it. Each turn, we will choose from the four operations with an equal probability `0.25`. If the remaining volume of soup is not enough to complete the operation, we will serve as much as possible. We stop once we no longer have some quantity of both types of soup.

**Note** that we do not have an operation where all `100` ml's of **soup B** are used first.

Return _the probability that **soup A** will be empty first, plus half the probability that **A** and **B** become empty at the same time_. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

**Input:** n = 50

**Output:** 0.62500

**Explanation:** If we choose the first two operations, A will become empty first.

For the third operation, A and B will become empty at the same time.

For the fourth operation, B will become empty first.

So the total probability of A becoming empty first plus half the probability that A and B become empty at the same time, is 0.25 \* (1 + 1 + 0.5 + 0) = 0.625.

**Example 2:**

**Input:** n = 100

**Output:** 0.71875

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun soupServings(n: Int): Double {
        return solve(n)
    }

    private fun solve(n: Int): Double {
        var n = n
        n = n / 25 + if (n % 25 > 0) 1 else 0
        return if (n >= 500) {
            1.0
        } else {
            find(n, n, Array(n + 1) { arrayOfNulls(n + 1) })
        }
    }

    private fun find(a: Int, b: Int, mem: Array<Array<Double?>>): Double {
        if (a <= 0 && b <= 0) {
            return 0.5
        } else if (a <= 0) {
            return 1.0
        } else if (b <= 0) {
            return 0.0
        }
        if (mem[a][b] != null) {
            return mem[a][b]!!
        }
        var prob: Double = find(a - 4, b, mem)
        prob += find(a - 3, b - 1, mem)
        prob += find(a - 2, b - 2, mem)
        prob += find(a - 1, b - 3, mem)
        mem[a][b] = 0.25 * prob
        return mem[a][b]!!
    }
}
```