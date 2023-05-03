[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 945\. Minimum Increment to Make Array Unique

Medium

You are given an integer array `nums`. In one move, you can pick an index `i` where `0 <= i < nums.length` and increment `nums[i]` by `1`.

Return _the minimum number of moves to make every value in_ `nums` _**unique**_.

The test cases are generated so that the answer fits in a 32-bit integer.

**Example 1:**

**Input:** nums = [1,2,2]

**Output:** 1

**Explanation:** After 1 move, the array could be [1, 2, 3].

**Example 2:**

**Input:** nums = [3,2,1,2,1,7]

**Output:** 6

**Explanation:** After 6 moves, the array could be [3, 4, 1, 2, 5, 7]. It can be shown with 5 or less moves that it is impossible for the array to have all unique values.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun minIncrementForUnique(nums: IntArray): Int {
        var max = 0
        for (num in nums) {
            max = Math.max(max, num)
        }
        val counts = IntArray(nums.size + max)
        for (num in nums) {
            counts[num]++
        }
        var minMoves = 0
        for (i in counts.indices) {
            if (counts[i] <= 1) {
                continue
            }
            val remaining = counts[i] - 1
            minMoves += remaining
            counts[i + 1] = counts[i + 1] + remaining
        }
        return minMoves
    }
}
```