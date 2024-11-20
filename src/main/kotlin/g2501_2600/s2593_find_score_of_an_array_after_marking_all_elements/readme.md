[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2593\. Find Score of an Array After Marking All Elements

Medium

You are given an array `nums` consisting of positive integers.

Starting with `score = 0`, apply the following algorithm:

*   Choose the smallest integer of the array that is not marked. If there is a tie, choose the one with the smallest index.
*   Add the value of the chosen integer to `score`.
*   Mark **the chosen element and its two adjacent elements if they exist**.
*   Repeat until all the array elements are marked.

Return _the score you get after applying the above algorithm_.

**Example 1:**

**Input:** nums = [2,1,3,4,5,2]

**Output:** 7

**Explanation:** We mark the elements as follows: 
- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [<ins>2</ins>,<ins>1</ins>,<ins>3</ins>,4,5,2]. 
- 2 is the smallest unmarked element, so we mark it and its left adjacent element: [<ins>2</ins>,<ins>1</ins>,<ins>3</ins>,4,<ins>5</ins>,<ins>2</ins>]. 
- 4 is the only remaining unmarked element, so we mark it: [<ins>2</ins>,<ins>1</ins>,<ins>3</ins>,<ins>4</ins>,<ins>5</ins>,<ins>2</ins>]. Our score is 1 + 2 + 4 = 7.

**Example 2:**

**Input:** nums = [2,3,5,1,3,2]

**Output:** 5

**Explanation:** We mark the elements as follows: 
- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [2,3,<ins>5</ins>,<ins>1</ins>,<ins>3</ins>,2]. 
- 2 is the smallest unmarked element, since there are two of them, we choose the left-most one, so we mark the one at index 0 and its right adjacent element: [<ins>2</ins>,<ins>3</ins>,<ins>5</ins>,<ins>1</ins>,<ins>3</ins>,2]. 
- 2 is the only remaining unmarked element, so we mark it: [<ins>2</ins>,<ins>3</ins>,<ins>5</ins>,<ins>1</ins>,<ins>3</ins>,<ins>2</ins>]. Our score is 1 + 2 + 2 = 5.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun findScore(nums: IntArray): Long {
        var ans: Long = 0
        val pq = PriorityQueue { a: IntArray, b: IntArray ->
            if (a[0] == b[0]
            ) {
                a[1] - b[1]
            } else {
                a[0] - b[0]
            }
        }
        val vis = BooleanArray(nums.size)
        for (i in nums.indices) {
            pq.offer(intArrayOf(nums[i], i))
        }
        while (pq.isNotEmpty()) {
            val it = pq.poll()
            if (!vis[it[1]]) {
                vis[it[1]] = true
                ans += it[0].toLong()
                if (it[1] > 0) {
                    vis[it[1] - 1] = true
                }
                if (it[1] < nums.size - 1) {
                    vis[it[1] + 1] = true
                }
            }
        }
        return ans
    }
}
```