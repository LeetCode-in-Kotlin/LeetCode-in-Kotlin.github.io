[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1467\. Probability of a Two Boxes Having The Same Number of Distinct Balls

Hard

Given `2n` balls of `k` distinct colors. You will be given an integer array `balls` of size `k` where `balls[i]` is the number of balls of color `i`.

All the balls will be **shuffled uniformly at random**, then we will distribute the first `n` balls to the first box and the remaining `n` balls to the other box (Please read the explanation of the second example carefully).

Please note that the two boxes are considered different. For example, if we have two balls of colors `a` and `b`, and two boxes `[]` and `()`, then the distribution `[a] (b)` is considered different than the distribution `[b] (a)` (Please read the explanation of the first example carefully).

Return _the probability_ that the two boxes have the same number of distinct balls. Answers within <code>10<sup>-5</sup></code> of the actual value will be accepted as correct.

**Example 1:**

**Input:** balls = [1,1]

**Output:** 1.00000

**Explanation:** Only 2 ways to divide the balls equally: 

- A ball of color 1 to box 1 and a ball of color 2 to box 2 

- A ball of color 2 to box 1 and a ball of color 1 to box 2
  
In both ways, the number of distinct colors in each box is equal. The probability is 2/2 = 1

**Example 2:**

**Input:** balls = [2,1,1]

**Output:** 0.66667

**Explanation:** We have the set of balls [1, 1, 2, 3] 

This set of balls will be shuffled randomly and we may have one of the 12 distinct shuffles with equal probability (i.e. 1/12): 

[1,1 / 2,3], [1,1 / 3,2], [1,2 / 1,3], [1,2 / 3,1], [1,3 / 1,2], [1,3 / 2,1], [2,1 / 1,3], [2,1 / 3,1], [2,3 / 1,1], [3,1 / 1,2], [3,1 / 2,1], [3,2 / 1,1] 

After that, we add the first two balls to the first box and the second two balls to the second box. We can see that 8 of these 12 possible random distributions have the same number of distinct colors of balls in each box. 

Probability is 8/12 = 0.66667

**Example 3:**

**Input:** balls = [1,2,1,2]

**Output:** 0.60000

**Explanation:** The set of balls is [1, 2, 2, 3, 4, 4]. It is hard to display all the 180 possible random shuffles of this set but it is easy to check that 108 of them will have the same number of distinct colors in each box. Probability = 108 / 180 = 0.6

**Constraints:**

*   `1 <= balls.length <= 8`
*   `1 <= balls[i] <= 6`
*   `sum(balls)` is even.

## Solution

```kotlin
class Solution {
    fun getProbability(balls: IntArray): Double {
        val m = balls.size
        var s = 0
        for (b in balls) {
            s += b
        }
        val c = Array(s + 1) { DoubleArray(s / 2 + 1) }
        c[0][0] = 1.0
        for (i in 1 until s + 1) {
            c[i][0] = 1.0
            for (j in 1 until s / 2 + 1) {
                c[i][j] = c[i - 1][j] + c[i - 1][j - 1]
            }
        }
        var dp = Array(2 * m + 1) { DoubleArray(s / 2 + 1) }
        dp[m][0] = 1.0
        var sum = 0
        for (b in balls) {
            sum += b
            val ndp = Array(2 * m + 1) { DoubleArray(s / 2 + 1) }
            for (i in 0..b) {
                for (j in 0 until 2 * m + 1) {
                    for (k in 0 until s / 2 + 1) {
                        if (dp[j][k] == 0.0) {
                            continue
                        }
                        val nk = k + i
                        val nr = sum - nk
                        if (nk <= s / 2 && nr <= s / 2) {
                            val i1 = if (i == b) j + 1 else j
                            val nj = if (i == 0) j - 1 else i1
                            ndp[nj][nk] += dp[j][k] * c[b][i]
                        }
                    }
                }
            }
            dp = ndp
        }
        return dp[m][s / 2] / c[s][s / 2]
    }
}
```