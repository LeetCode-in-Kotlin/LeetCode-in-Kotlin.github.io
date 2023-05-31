[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 128\. Longest Consecutive Sequence

Medium

Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

**Input:** nums = [100,4,200,1,3,2]

**Output:** 4

**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

**Example 2:**

**Input:** nums = [0,3,7,2,5,8,4,6,0,1]

**Output:** 9

**Constraints:**

*   <code>0 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun longestConsecutive(nums: IntArray): Int {
        if (nums.isEmpty()) {
            return 0
        }
        val queue = PriorityQueue<Int>()
        nums.forEach {
            queue.add(it)
        }
        var lastNum = Integer.MIN_VALUE
        var length = 0
        var maxLength = 1
        while (queue.isNotEmpty()) {
            val num = queue.poll()
            if (num == lastNum) {
                continue
            }
            length ++
            if (num - lastNum > 1) {
                length = 1
            }
            lastNum = num
            maxLength = maxOf(maxLength, length)
        }
        return maxLength
    }
}
```