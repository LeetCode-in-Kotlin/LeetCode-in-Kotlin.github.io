[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1438\. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit

Medium

Given an array of integers `nums` and an integer `limit`, return the size of the longest **non-empty** subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`_._

**Example 1:**

**Input:** nums = [8,2,4,7], limit = 4

**Output:** 2

**Explanation:** All subarrays are: 

[8] with maximum absolute diff \|8-8\| = 0 <= 4.

[8,2] with maximum absolute diff \|8-2\| = 6 > 4. 

[8,2,4] with maximum absolute diff \|8-2\| = 6 > 4. 

[8,2,4,7] with maximum absolute diff \|8-2\| = 6 > 4. 

[2] with maximum absolute diff \|2-2\| = 0 <= 4.

[2,4] with maximum absolute diff \|2-4\| = 2 <= 4. 

[2,4,7] with maximum absolute diff \|2-7\| = 5 > 4. 

[4] with maximum absolute diff \|4-4\| = 0 <= 4. 

[4,7] with maximum absolute diff \|4-7\| = 3 <= 4. 

[7] with maximum absolute diff \|7-7\| = 0 <= 4. 

Therefore, the size of the longest subarray is 2.

**Example 2:**

**Input:** nums = [10,1,2,4,7,2], limit = 5

**Output:** 4

**Explanation:** The subarray [2,4,7,2] is the longest since the maximum absolute diff is \|2-7\| = 5 <= 5.

**Example 3:**

**Input:** nums = [4,2,2,2,4,4,2,2], limit = 0

**Output:** 3

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= limit <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.ArrayDeque

class Solution {
    fun longestSubarray(nums: IntArray, limit: Int): Int {
        val maxQ = ArrayDeque<Int>()
        val minQ = ArrayDeque<Int>()
        var best = 0
        var left = 0
        for (right in nums.indices) {
            while (maxQ.isNotEmpty() && nums[right] > nums[maxQ.peekLast()]) {
                maxQ.removeLast()
            }
            maxQ.offerLast(right)
            while (minQ.isNotEmpty() && nums[right] < nums[minQ.peekLast()]) {
                minQ.removeLast()
            }
            minQ.offerLast(right)
            while (nums[maxQ.peekFirst()] - nums[minQ.peekFirst()] > limit) {
                if (maxQ.peekFirst() == left) {
                    maxQ.removeFirst()
                }
                if (minQ.peekFirst() == left) {
                    minQ.removeFirst()
                }
                left++
            }
            best = Math.max(best, right - left + 1)
        }
        return best
    }
}
```