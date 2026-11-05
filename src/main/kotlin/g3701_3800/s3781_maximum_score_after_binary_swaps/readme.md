[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3781\. Maximum Score After Binary Swaps

Medium

You are given an integer array `nums` of length `n` and a binary string `s` of the same length.

Initially, your score is 0. Each index `i` where `s[i] = '1'` contributes `nums[i]` to the score.

You may perform **any** number of operations (including zero). In one operation, you may choose an index `i` such that `0 <= i < n - 1`, where `s[i] = '0'`, and `s[i + 1] = '1'`, and swap these two characters.

Return an integer denoting the **maximum possible score** you can achieve.

**Example 1:**

**Input:** nums = [2,1,5,2,3], s = "01010"

**Output:** 7

**Explanation:**

We can perform the following swaps:

*   Swap at index `i = 0`: `"01010"` changes to `"10010"`
*   Swap at index `i = 2`: `"10010"` changes to `"10100"`

Positions 0 and 2 contain `'1'`, contributing `nums[0] + nums[2] = 2 + 5 = 7`. This is maximum score achievable.

**Example 2:**

**Input:** nums = [4,7,2,9], s = "0000"

**Output:** 0

**Explanation:**

There are no `'1'` characters in `s`, so no swaps can be performed. The score remains 0.

**Constraints:**

*   `n == nums.length == s.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `s[i]` is either `'0'` or `'1'`

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun maximumScore(nums: IntArray, s: String): Long {
        var sum: Long = 0
        val pq = PriorityQueue<Int>(Comparator { a: Int?, b: Int? -> Integer.compare(b!!, a!!) })
        for (i in nums.indices) {
            if (s[i] == '1') {
                if (pq.isEmpty() || pq.peek()!! <= nums[i]) {
                    sum += nums[i].toLong()
                } else {
                    sum += pq.poll()!!.toLong()
                    pq.add(nums[i])
                }
            } else {
                pq.add(nums[i])
            }
        }
        return sum
    }
}
```