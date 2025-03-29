[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3321\. Find X-Sum of All K-Long Subarrays II

Hard

You are given an array `nums` of `n` integers and two integers `k` and `x`.

The **x-sum** of an array is calculated by the following procedure:

*   Count the occurrences of all elements in the array.
*   Keep only the occurrences of the top `x` most frequent elements. If two elements have the same number of occurrences, the element with the **bigger** value is considered more frequent.
*   Calculate the sum of the resulting array.

**Note** that if an array has less than `x` distinct elements, its **x-sum** is the sum of the array.

Return an integer array `answer` of length `n - k + 1` where `answer[i]` is the **x-sum** of the subarray `nums[i..i + k - 1]`.

**Example 1:**

**Input:** nums = [1,1,2,2,3,4,2,3], k = 6, x = 2

**Output:** [6,10,12]

**Explanation:**

*   For subarray `[1, 1, 2, 2, 3, 4]`, only elements 1 and 2 will be kept in the resulting array. Hence, `answer[0] = 1 + 1 + 2 + 2`.
*   For subarray `[1, 2, 2, 3, 4, 2]`, only elements 2 and 4 will be kept in the resulting array. Hence, `answer[1] = 2 + 2 + 2 + 4`. Note that 4 is kept in the array since it is bigger than 3 and 1 which occur the same number of times.
*   For subarray `[2, 2, 3, 4, 2, 3]`, only elements 2 and 3 are kept in the resulting array. Hence, `answer[2] = 2 + 2 + 2 + 3 + 3`.

**Example 2:**

**Input:** nums = [3,8,7,8,7,5], k = 2, x = 2

**Output:** [11,15,15,15,12]

**Explanation:**

Since `k == x`, `answer[i]` is equal to the sum of the subarray `nums[i..i + k - 1]`.

**Constraints:**

*   `nums.length == n`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= x <= k <= nums.length`

## Solution

```kotlin
import java.util.HashMap
import java.util.TreeSet

class Solution {
    private class RC(v: Int, c: Int) : Comparable<RC> {
        var `val`: Int
        var cnt: Int

        init {
            `val` = v
            cnt = c
        }

        override fun compareTo(other: RC): Int {
            if (cnt != other.cnt) {
                return cnt - other.cnt
            }
            return `val` - other.`val`
        }
    }

    fun findXSum(nums: IntArray, k: Int, x: Int): LongArray {
        val n = nums.size
        val ans = LongArray(n - k + 1)
        val cnt: MutableMap<Int, Int> = HashMap<Int, Int>()
        val s1 = TreeSet<RC>()
        val s2 = TreeSet<RC>()
        var sum: Long = 0
        var xSum: Long = 0
        for (i in 0 until n) {
            sum += nums[i].toLong()
            var curCnt: Int = cnt.getOrDefault(nums[i], 0)
            cnt.put(nums[i], curCnt + 1)
            var tmp = RC(nums[i], curCnt)
            if (s1.contains(tmp)) {
                s1.remove(tmp)
                s1.add(RC(nums[i], curCnt + 1))
                xSum += nums[i].toLong()
            } else {
                s2.remove(tmp)
                s1.add(RC(nums[i], curCnt + 1))
                xSum += nums[i].toLong() * (curCnt + 1)
                while (s1.size > x) {
                    val l = s1.first()
                    s1.remove(l)
                    xSum -= l.`val`.toLong() * l.cnt
                    s2.add(l)
                }
            }
            if (i >= k - 1) {
                ans[i - k + 1] = if (s1.size == x) xSum else sum
                val v = nums[i - k + 1]
                sum -= v.toLong()
                curCnt = cnt[v]!!
                if (curCnt > 1) {
                    cnt.put(v, curCnt - 1)
                } else {
                    cnt.remove(v)
                }
                tmp = RC(v, curCnt)
                if (s2.contains(tmp)) {
                    s2.remove(tmp)
                    if (curCnt > 1) {
                        s2.add(RC(v, curCnt - 1))
                    }
                } else {
                    s1.remove(tmp)
                    xSum -= v.toLong() * curCnt
                    if (curCnt > 1) {
                        s2.add(RC(v, curCnt - 1))
                    }
                    while (s1.size < x && s2.isNotEmpty()) {
                        val r = s2.last()
                        s2.remove(r)
                        s1.add(r)
                        xSum += r.`val`.toLong() * r.cnt
                    }
                }
            }
        }
        return ans
    }
}
```