[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 368\. Largest Divisible Subset

Medium

Given a set of **distinct** positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

*   `answer[i] % answer[j] == 0`, or
*   `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** [1,2]

**Explanation:** [1,3] is also accepted.

**Example 2:**

**Input:** nums = [1,2,4,8]

**Output:** [1,2,4,8]

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 2 * 10<sup>9</sup></code>
*   All the integers in `nums` are **unique**.

## Solution

```kotlin
class Solution {
    fun largestDivisibleSubset(nums: IntArray): List<Int> {
        val n = nums.size
        val count = IntArray(n)
        val pre = IntArray(n)
        nums.sort()
        var max = 0
        var index = -1
        for (i in 0 until n) {
            count[i] = 1
            pre[i] = -1
            for (j in i - 1 downTo 0) {
                if (nums[i] % nums[j] == 0 && 1 + count[j] > count[i]) {
                    count[i] = count[j] + 1
                    pre[i] = j
                }
            }
            if (count[i] > max) {
                max = count[i]
                index = i
            }
        }
        val res: MutableList<Int> = ArrayList()
        while (index != -1) {
            res.add(nums[index])
            index = pre[index]
        }
        return res
    }
}
```