[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3629\. Minimum Jumps to Reach End via Prime Teleportation

Medium

You are given an integer array `nums` of length `n`.

You start at index 0, and your goal is to reach index `n - 1`.

From any index `i`, you may perform one of the following operations:

*   **Adjacent Step**: Jump to index `i + 1` or `i - 1`, if the index is within bounds.
*   **Prime Teleportation**: If `nums[i]` is a prime number `p`, you may instantly jump to any index `j != i` such that `nums[j] % p == 0`.

Return the **minimum** number of jumps required to reach index `n - 1`.

**Example 1:**

**Input:** nums = [1,2,4,6]

**Output:** 2

**Explanation:**

One optimal sequence of jumps is:

*   Start at index `i = 0`. Take an adjacent step to index 1.
*   At index `i = 1`, `nums[1] = 2` is a prime number. Therefore, we teleport to index `i = 3` as `nums[3] = 6` is divisible by 2.

Thus, the answer is 2.

**Example 2:**

**Input:** nums = [2,3,4,7,9]

**Output:** 2

**Explanation:**

One optimal sequence of jumps is:

*   Start at index `i = 0`. Take an adjacent step to index `i = 1`.
*   At index `i = 1`, `nums[1] = 3` is a prime number. Therefore, we teleport to index `i = 4` since `nums[4] = 9` is divisible by 3.

Thus, the answer is 2.

**Example 3:**

**Input:** nums = [4,6,5,8]

**Output:** 3

**Explanation:**

*   Since no teleportation is possible, we move through `0 → 1 → 2 → 3`. Thus, the answer is 3.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
import java.util.ArrayDeque
import kotlin.math.max

class Solution {
    fun minJumps(nums: IntArray): Int {
        val n = nums.size
        if (n == 1) {
            return 0
        }
        var maxVal = 0
        for (v in nums) {
            maxVal = max(maxVal, v)
        }
        val isPrime = sieve(maxVal)
        val posOfValue: Array<ArrayList<Int>> = Array<ArrayList<Int>>(maxVal + 1) { ArrayList<Int>() }
        for (i in 0..<n) {
            val v = nums[i]
            posOfValue[v].add(i)
        }
        val primeProcessed = BooleanArray(maxVal + 1)
        val dist = IntArray(n)
        dist.fill(-1)
        val q = ArrayDeque<Int>()
        q.add(0)
        dist[0] = 0
        while (q.isNotEmpty()) {
            val i: Int = q.poll()!!
            val d = dist[i]
            if (i == n - 1) {
                return d
            }
            if (i + 1 < n && dist[i + 1] == -1) {
                dist[i + 1] = d + 1
                q.add(i + 1)
            }
            if (i - 1 >= 0 && dist[i - 1] == -1) {
                dist[i - 1] = d + 1
                q.add(i - 1)
            }
            val v = nums[i]
            if (v <= maxVal && isPrime[v] && !primeProcessed[v]) {
                var mult = v
                while (mult <= maxVal) {
                    val list = posOfValue[mult]
                    for (idx in list) {
                        if (dist[idx] == -1) {
                            dist[idx] = d + 1
                            q.add(idx)
                        }
                    }
                    mult += v
                }
                primeProcessed[v] = true
            }
        }
        return -1
    }

    private fun sieve(n: Int): BooleanArray {
        val prime = BooleanArray(n + 1)
        if (n >= 2) {
            prime.fill(true)
        }
        if (n >= 0) {
            prime[0] = false
        }
        if (n >= 1) {
            prime[1] = false
        }
        var i = 2
        while (i.toLong() * i <= n) {
            if (prime[i]) {
                var j = i * i
                while (j <= n) {
                    prime[j] = false
                    j += i
                }
            }
            i++
        }
        return prime
    }
}
```