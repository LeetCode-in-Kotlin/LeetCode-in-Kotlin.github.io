[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2099\. Find Subsequence of Length K With the Largest Sum

Easy

You are given an integer array `nums` and an integer `k`. You want to find a **subsequence** of `nums` of length `k` that has the **largest** sum.

Return _**any** such subsequence as an integer array of length_ `k`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [2,1,3,3], k = 2

**Output:** [3,3]

**Explanation:** The subsequence has the largest sum of 3 + 3 = 6.

**Example 2:**

**Input:** nums = [-1,-2,3,4], k = 3

**Output:** [-1,3,4]

**Explanation:** The subsequence has the largest sum of -1 + 3 + 4 = 6. 

**Example 3:**

**Input:** nums = [3,4,3,3], k = 2

**Output:** [3,4]

**Explanation:** The subsequence has the largest sum of 3 + 4 = 7.

Another possible subsequence is [4, 3]. 

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= nums.length`

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun maxSubsequence(nums: IntArray, k: Int): IntArray {
        // Create proirity queue with min priority queue so that min element will be removed first,
        // with index
        // Add those unique index in a set
        // Loop from 0 to n-1 and add element in result if set contains those index
        // For ex. set has index 3,5,6 Just add those element. Order will be maintained
        // We are defining the min priority queue
        val q = PriorityQueue { a: IntArray, b: IntArray -> a[0] - b[0] }
        // Add element with index to priority queue
        for (i in nums.indices) {
            q.offer(intArrayOf(nums[i], i))
            if (q.size > k) {
                q.poll()
            }
        }
        // Set to keep index
        val index: MutableSet<Int> = HashSet()
        // At the index in the set since index are unique
        while (q.isNotEmpty()) {
            val top = q.poll()
            index.add(top[1])
        }
        // Final result add here
        val result = IntArray(k)
        // Just add the element in the result for those index present in SET
        var p = 0
        for (i in nums.indices) {
            if (index.contains(i)) {
                result[p] = nums[i]
                ++p
            }
        }
        return result
    }
}
```