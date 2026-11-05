[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3984\. Divisible Game

Medium

You are given an integer array `nums` of length `n`.

Alice and Bob are playing a game. Alice chooses:

*   An integer `k` such that `k > 1`.
*   Two integers `l` and `r` such that `0 <= l <= r < n`.

Initially, both Alice's and Bob's scores are 0.

For each index `i` in the range `[l, r]` (inclusive):

*   If `nums[i]` is divisible by `k`, Alice's score **increases** by `nums[i]`.
*   Otherwise, Bob's score **increases** by `nums[i]`.

The **score difference** is Alice's score **minus** Bob's score.

Alice wants to **maximize** the score difference. If there are multiple values of `k` that achieve the **maximum** score difference, she chooses the **smallest** such `k`.

Return the **product** of the **maximum** score difference and the chosen value of `k`. Since the result can be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,4,6,8]

**Output:** 36

**Explanation:**

*   Alice can choose `k = 2`, `l = 1`, and `r = 3`.
*   All values in `nums[1..3]` are divisible by 2, so Alice's score is `4 + 6 + 8 = 18`, while Bob's score is 0.
*   The score difference is 18, which is the maximum possible. Among all values of `k` that achieve this score difference, the smallest is 2.
*   Therefore, the answer is `18 * 2 = 36`.

**Example 2:**

**Input:** nums = [2,1,2]

**Output:** 6

**Explanation:**

*   Alice can choose `k = 2`, `l = 0`, and `r = 2`.
*   The values `nums[0]` and `nums[2]` are divisible by 2, so Alice's score is `2 + 2 = 4`. The value `nums[1]` is not divisible by 2, so Bob's score is 1.
*   The score difference is `4 - 1 = 3`, which is the maximum possible. Among all values of `k` that achieve this score difference, the smallest is 2.
*   Therefore, the answer is `3 * 2 = 6`.

**Example 3:**

**Input:** nums = [1]

**Output:** 1000000005

**Explanation:**

*   Alice must choose some `k > 1`. The smallest possible choice is `k = 2`.
*   Since `nums[0]` is not divisible by 2, Alice's score is 0, while Bob's score is 1.
*   The score difference is -1, which is the maximum possible.
*   Therefore, the answer is `-1 * 2 = -2`. Modulo <code>10<sup>9</sup> + 7</code>, this equals 1000000005.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun divisibleGame(nums: IntArray): Int {
        val prime = HashSet<Int?>()
        val mod = 1e9.toInt() + 7
        var bestscore = nums[0].toLong()
        for (i in nums) {
            bestscore = min(bestscore, i.toLong())
        }
        bestscore = -bestscore
        var bestprime: Long = 2
        for (i in nums) {
            var i = i
            var j = 2
            while (j * j <= i) {
                if (i % j == 0) {
                    prime.add(j)
                    while (i % j == 0) {
                        i = i / j
                    }
                }
                j++
            }
            if (i > 1) {
                prime.add(i)
            }
        }
        for (p in prime) {
            var curr: Long = 0
            var maxi = Long.MIN_VALUE
            for (i in nums) {
                val `val` = (if (i % p!! == 0) i else -i).toLong()
                curr = max(0, curr + `val`)
                maxi = max(maxi, curr)
            }
            if (maxi > bestscore) {
                bestscore = maxi
                bestprime = p!!.toLong()
            } else if (maxi == bestscore && p!! < bestprime) {
                bestprime = p.toLong()
            }
        }
        bestscore = ((bestscore % mod) + mod) % mod
        return ((bestscore * (bestprime % mod)) % mod).toInt()
    }
}
```