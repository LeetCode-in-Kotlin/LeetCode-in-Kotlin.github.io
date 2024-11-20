[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3266\. Final Array State After K Multiplication Operations II

Hard

You are given an integer array `nums`, an integer `k`, and an integer `multiplier`.

You need to perform `k` operations on `nums`. In each operation:

*   Find the **minimum** value `x` in `nums`. If there are multiple occurrences of the minimum value, select the one that appears **first**.
*   Replace the selected minimum value `x` with `x * multiplier`.

After the `k` operations, apply **modulo** <code>10<sup>9</sup> + 7</code> to every value in `nums`.

Return an integer array denoting the _final state_ of `nums` after performing all `k` operations and then applying the modulo.

**Example 1:**

**Input:** nums = [2,1,3,5,6], k = 5, multiplier = 2

**Output:** [8,4,6,5,6]

**Explanation:**

| Operation               | Result           |
|-------------------------|------------------|
| After operation 1       | [2, 2, 3, 5, 6]  |
| After operation 2       | [4, 2, 3, 5, 6]  |
| After operation 3       | [4, 4, 3, 5, 6]  |
| After operation 4       | [4, 4, 6, 5, 6]  |
| After operation 5       | [8, 4, 6, 5, 6]  |
| After applying modulo   | [8, 4, 6, 5, 6]  |

**Example 2:**

**Input:** nums = [100000,2000], k = 2, multiplier = 1000000

**Output:** [999999307,999999993]

**Explanation:**

| Operation               | Result               |
|-------------------------|----------------------|
| After operation 1       | [100000, 2000000000] |
| After operation 2       | [100000000000, 2000000000] |
| After applying modulo   | [999999307, 999999993] |

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>
*   <code>1 <= multiplier <= 10<sup>6</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue
import kotlin.math.max

@Suppress("NAME_SHADOWING")
class Solution {
    fun getFinalState(nums: IntArray, k: Int, multiplier: Int): IntArray {
        var k = k
        if (multiplier == 1) {
            return nums
        }
        val n = nums.size
        var mx = 0
        for (x in nums) {
            mx = max(mx, x)
        }
        val a = LongArray(n)
        var left = k
        var shouldExit = false
        run {
            var i = 0
            while (i < n && !shouldExit) {
                var x = nums[i].toLong()
                while (x < mx) {
                    x *= multiplier.toLong()
                    if (--left < 0) {
                        shouldExit = true
                        break
                    }
                }
                a[i] = x
                i++
            }
        }
        if (left < 0) {
            val pq =
                PriorityQueue { p: LongArray, q: LongArray ->
                    if (p[0] != q[0]
                    ) {
                        java.lang.Long.compare(p[0], q[0])
                    } else {
                        java.lang.Long.compare(p[1], q[1])
                    }
                }
            for (i in 0 until n) {
                pq.offer(longArrayOf(nums[i].toLong(), i.toLong()))
            }
            while (k-- > 0) {
                val p = pq.poll()
                p[0] *= multiplier.toLong()
                pq.offer(p)
            }
            while (pq.isNotEmpty()) {
                val p = pq.poll()
                nums[p[1].toInt()] = (p[0] % MOD).toInt()
            }
            return nums
        }

        val ids: Array<Int> = Array(n) { i: Int -> i }
        ids.sortWith { i: Int?, j: Int? -> java.lang.Long.compare(a[i!!], a[j!!]) }
        k = left
        val pow1 = pow(multiplier.toLong(), k / n)
        val pow2 = pow1 * multiplier % MOD
        for (i in 0 until n) {
            val j = ids[i]
            nums[j] = (a[j] % MOD * (if (i < k % n) pow2 else pow1) % MOD).toInt()
        }
        return nums
    }

    private fun pow(x: Long, n: Int): Long {
        var x = x
        var n = n
        var res: Long = 1
        while (n > 0) {
            if (n % 2 > 0) {
                res = res * x % MOD
            }
            x = x * x % MOD
            n /= 2
        }
        return res
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```