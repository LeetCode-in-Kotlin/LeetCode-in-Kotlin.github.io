[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3318\. Find X-Sum of All K-Long Subarrays I

Easy

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

*   `1 <= n == nums.length <= 50`
*   `1 <= nums[i] <= 50`
*   `1 <= x <= k <= nums.length`

## Solution

```kotlin
import java.util.Comparator
import java.util.HashMap
import java.util.PriorityQueue

class Solution {
    private class Pair(num: Int, freq: Int) {
        var num: Int
        var freq: Int

        init {
            this.num = num
            this.freq = freq
        }
    }

    fun findXSum(nums: IntArray, k: Int, x: Int): IntArray {
        val n = nums.size
        val ans = IntArray(n - k + 1)
        for (i in 0 until n - k + 1) {
            val map = HashMap<Int?, Int?>()
            val pq =
                PriorityQueue<Pair>(
                    Comparator { a: Pair, b: Pair ->
                        if (a.freq == b.freq) {
                            return@Comparator b.num - a.num
                        }
                        b.freq - a.freq
                    }
                )
            for (j in i until i + k) {
                map.put(nums[j], map.getOrDefault(nums[j], 0)!! + 1)
            }
            for (entry in map.entries) {
                pq.add(Pair(entry.key!!, entry.value!!))
            }
            var count = x
            var sum = 0
            while (pq.isNotEmpty() && count > 0) {
                val pair = pq.remove()
                sum += pair.num * pair.freq
                count--
            }
            ans[i] = sum
        }
        return ans
    }
}
```