[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 503\. Next Greater Element II

Medium

Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return _the **next greater number** for every element in_ `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

**Example 1:**

**Input:** nums = [1,2,1]

**Output:** [2,-1,2] Explanation: The first 1's next greater number is 2; The number 2 can't find next greater number. The second 1's next greater number needs to search circularly, which is also 2.

**Example 2:**

**Input:** nums = [1,2,3,4,3]

**Output:** [2,3,4,-1,4]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque

class Solution {
    fun nextGreaterElements(nums: IntArray): IntArray {
        val result = IntArray(nums.size)
        val stack: Deque<Int> = ArrayDeque()
        for (i in nums.size * 2 - 1 downTo 0) {
            while (stack.isNotEmpty() && nums[stack.peek()] <= nums[i % nums.size]) {
                stack.pop()
            }
            result[i % nums.size] = if (stack.isEmpty()) -1 else nums[stack.peek()]
            stack.push(i % nums.size)
        }
        return result
    }
}
```