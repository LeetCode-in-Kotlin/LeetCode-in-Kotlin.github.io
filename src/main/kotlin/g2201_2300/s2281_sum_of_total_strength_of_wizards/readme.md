[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2281\. Sum of Total Strength of Wizards

Hard

As the ruler of a kingdom, you have an army of wizards at your command.

You are given a **0-indexed** integer array `strength`, where `strength[i]` denotes the strength of the <code>i<sup>th</sup></code> wizard. For a **contiguous** group of wizards (i.e. the wizards' strengths form a **subarray** of `strength`), the **total strength** is defined as the **product** of the following two values:

*   The strength of the **weakest** wizard in the group.
*   The **total** of all the individual strengths of the wizards in the group.

Return _the **sum** of the total strengths of **all** contiguous groups of wizards_. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** strength = [1,3,1,2]

**Output:** 44

**Explanation:** The following are all the contiguous groups of wizards:

- \[1] from [**1**,3,1,2] has a total strength of min([1]) \* sum([1]) = 1 \* 1 = 1

- \[3] from [1,**3**,1,2] has a total strength of min([3]) \* sum([3]) = 3 \* 3 = 9

- \[1] from [1,3,**1**,2] has a total strength of min([1]) \* sum([1]) = 1 \* 1 = 1

- \[2] from [1,3,1,**2**] has a total strength of min([2]) \* sum([2]) = 2 \* 2 = 4

- \[1,3] from [**1,3**,1,2] has a total strength of min([1,3]) \* sum([1,3]) = 1 \* 4 = 4

- \[3,1] from [1,**3,1**,2] has a total strength of min([3,1]) \* sum([3,1]) = 1 \* 4 = 4

- \[1,2] from [1,3,**1,2**] has a total strength of min([1,2]) \* sum([1,2]) = 1 \* 3 = 3

- \[1,3,1] from [**1,3,1**,2] has a total strength of min([1,3,1]) \* sum([1,3,1]) = 1 \* 5 = 5

- \[3,1,2] from [1,**3,1,2**] has a total strength of min([3,1,2]) \* sum([3,1,2]) = 1 \* 6 = 6

- \[1,3,1,2] from [**1,3,1,2**] has a total strength of min([1,3,1,2]) \* sum([1,3,1,2]) = 1 \* 7 = 7

The sum of all the total strengths is 1 + 9 + 1 + 4 + 4 + 4 + 3 + 5 + 6 + 7 = 44.

**Example 2:**

**Input:** strength = [5,4,6]

**Output:** 213

**Explanation:** The following are all the contiguous groups of wizards:

- \[5] from [**5**,4,6] has a total strength of min([5]) \* sum([5]) = 5 \* 5 = 25

- \[4] from [5,**4**,6] has a total strength of min([4]) \* sum([4]) = 4 \* 4 = 16

- \[6] from [5,4,**6**] has a total strength of min([6]) \* sum([6]) = 6 \* 6 = 36

- \[5,4] from [**5,4**,6] has a total strength of min([5,4]) \* sum([5,4]) = 4 \* 9 = 36

- \[4,6] from [5,**4,6**] has a total strength of min([4,6]) \* sum([4,6]) = 4 \* 10 = 40

- \[5,4,6] from [**5,4,6**] has a total strength of min([5,4,6]) \* sum([5,4,6]) = 4 \* 15 = 60

The sum of all the total strengths is 25 + 16 + 36 + 36 + 40 + 60 = 213.

**Constraints:**

*   <code>1 <= strength.length <= 10<sup>5</sup></code>
*   <code>1 <= strength[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

@Suppress("kotlin:S107")
class Solution {
    fun totalStrength(nums: IntArray): Int {
        val n = nums.size
        val forward = LongArray(n)
        val backward = LongArray(n)
        val prefix = LongArray(n + 1)
        val suffix = LongArray(n + 1)
        prefix[1] = nums[0].toLong()
        forward[0] = prefix[1]
        suffix[n - 1] = nums[n - 1].toLong()
        backward[n - 1] = suffix[n - 1]
        for (i in 1 until n) {
            forward[i] = nums[i] + forward[i - 1]
            prefix[i + 1] = prefix[i] + forward[i]
        }
        run {
            var i = n - 2
            while (0 <= i) {
                backward[i] = nums[i] + backward[i + 1]
                suffix[i] = suffix[i + 1] + backward[i]
                --i
            }
        }
        var res: Long = 0
        val dq: Deque<Int> = LinkedList()
        for (i in 0 until n) {
            while (dq.isNotEmpty() && nums[dq.peekLast()] >= nums[i]) {
                val cur = dq.pollLast()
                val prev = if (dq.isEmpty()) -1 else dq.peekLast()
                res = (
                    (
                        res +
                            getSum(
                                nums, forward, prefix, backward, suffix,
                                prev, cur, i,
                            ) *
                            nums[cur]
                        ) %
                        MOD
                    )
            }
            dq.add(i)
        }
        while (dq.isNotEmpty()) {
            val cur = dq.pollLast()
            val prev = if (dq.isEmpty()) -1 else dq.peekLast()
            res = (
                (
                    res +
                        getSum(nums, forward, prefix, backward, suffix, prev, cur, n) *
                        nums[cur]
                    ) %
                    MOD
                )
        }
        return res.toInt()
    }

    private fun getSum(
        nums: IntArray,
        forward: LongArray,
        prefix: LongArray,
        backward: LongArray,
        suffix: LongArray,
        prev: Int,
        cur: Int,
        next: Int,
    ): Long {
        val sum = (cur - prev) * nums[cur].toLong() % MOD * (next - cur) % MOD
        val preSum = getPresum(backward, suffix, prev + 1, cur - 1, next - cur)
        val postSum = getPostsum(forward, prefix, cur + 1, next - 1, cur - prev)
        return (sum + preSum + postSum) % MOD
    }

    private fun getPresum(backward: LongArray, suffix: LongArray, from: Int, to: Int, m: Int): Long {
        val n = backward.size
        val cnt = to - from + 1L
        return (
            (suffix[from] - suffix[to + 1] - cnt * (if (to + 1 == n) 0 else backward[to + 1]) % MOD) %
                MOD
                * m %
                MOD
            )
    }

    private fun getPostsum(forward: LongArray, prefix: LongArray, from: Int, to: Int, m: Int): Long {
        val cnt = to - from + 1L
        return (
            (prefix[to + 1] - prefix[from] - cnt * (if (0 == from) 0 else forward[from - 1]) % MOD) %
                MOD
                * m %
                MOD
            )
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```