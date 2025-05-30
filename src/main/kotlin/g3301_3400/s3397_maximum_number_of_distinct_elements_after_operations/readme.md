[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3397\. Maximum Number of Distinct Elements After Operations

Medium

You are given an integer array `nums` and an integer `k`.

You are allowed to perform the following **operation** on each element of the array **at most** _once_:

*   Add an integer in the range `[-k, k]` to the element.

Return the **maximum** possible number of **distinct** elements in `nums` after performing the **operations**.

**Example 1:**

**Input:** nums = [1,2,2,3,3,4], k = 2

**Output:** 6

**Explanation:**

`nums` changes to `[-1, 0, 1, 2, 3, 4]` after performing operations on the first four elements.

**Example 2:**

**Input:** nums = [4,4,4,4], k = 1

**Output:** 3

**Explanation:**

By adding -1 to `nums[0]` and 1 to `nums[1]`, `nums` changes to `[3, 5, 4, 4]`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxDistinctElements(nums: IntArray, k: Int): Int {
        nums.sort()
        var next = nums[0] - k + 1
        val n = nums.size
        var ans = 1
        for (i in 1..<n) {
            if (nums[i] + k < next) {
                continue
            }
            next = max(next, (nums[i] - k))
            ans++
            next++
        }
        return ans
    }
}
```