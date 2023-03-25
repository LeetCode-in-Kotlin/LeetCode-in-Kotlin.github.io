[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 740\. Delete and Earn

Medium

You are given an integer array `nums`. You want to maximize the number of points you get by performing the following operation any number of times:

*   Pick any `nums[i]` and delete it to earn `nums[i]` points. Afterwards, you must delete **every** element equal to `nums[i] - 1` and **every** element equal to `nums[i] + 1`.

Return _the **maximum number of points** you can earn by applying the above operation some number of times_.

**Example 1:**

**Input:** nums = [3,4,2]

**Output:** 6

**Explanation:** You can perform the following operations:

- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].

- Delete 2 to earn 2 points. nums = [].

You earn a total of 6 points.

**Example 2:**

**Input:** nums = [2,2,3,3,3,4]

**Output:** 9

**Explanation:** You can perform the following operations:

- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].

- Delete a 3 again to earn 3 points. nums = [3].

- Delete a 3 once more to earn 3 points. nums = [].

You earn a total of 9 points.

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun deleteAndEarn(nums: IntArray): Int {
        val sum = IntArray(10001)
        var min = 10001
        var max = 0
        for (num in nums) {
            sum[num] += num
            min = num.coerceAtMost(min)
            max = num.coerceAtLeast(max)
        }
        val dp = IntArray(max + 1)
        dp[min] = sum[min]
        for (i in min + 1..max) {
            dp[i] = dp[i - 1].coerceAtLeast(dp[i - 2] + sum[i])
        }
        return dp[max]
    }
}
```