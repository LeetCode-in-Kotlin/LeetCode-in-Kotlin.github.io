[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 456\. 132 Pattern

Medium

Given an array of `n` integers `nums`, a **132 pattern** is a subsequence of three integers `nums[i]`, `nums[j]` and `nums[k]` such that `i < j < k` and `nums[i] < nums[k] < nums[j]`.

Return `true` _if there is a **132 pattern** in_ `nums`_, otherwise, return_ `false`_._

**Example 1:**

**Input:** nums = [1,2,3,4]

**Output:** false

**Explanation:** There is no 132 pattern in the sequence.

**Example 2:**

**Input:** nums = [3,1,4,2]

**Output:** true

**Explanation:** There is a 132 pattern in the sequence: [1, 4, 2].

**Example 3:**

**Input:** nums = [-1,3,2,0]

**Output:** true

**Explanation:** There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 2 * 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class Solution {
    /*
     * It scans only once, this is the power of using correct data structure.
     * It goes from the right to the left.
     * It keeps pushing elements into the stack,
     * but it also keeps poping elements out of the stack as long as the current element is bigger than this number.
     */
    fun find132pattern(nums: IntArray): Boolean {
        val stack: Deque<Int> = LinkedList()
        var s3 = Int.MIN_VALUE
        for (i in nums.indices.reversed()) {
            if (nums[i] < s3) {
                return true
            } else {
                while (stack.isNotEmpty() && nums[i] > stack.peek()) {
                    s3 = Math.max(s3, stack.pop())
                }
            }
            stack.push(nums[i])
        }
        return false
    }
}
```