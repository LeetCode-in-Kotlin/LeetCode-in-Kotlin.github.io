[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2134\. Minimum Swaps to Group All 1's Together II

Medium

A **swap** is defined as taking two **distinct** positions in an array and swapping the values in them.

A **circular** array is defined as an array where we consider the **first** element and the **last** element to be **adjacent**.

Given a **binary** **circular** array `nums`, return _the minimum number of swaps required to group all_ `1`_'s present in the array together at **any location**_.

**Example 1:**

**Input:** nums = [0,1,0,1,1,0,0]

**Output:** 1

**Explanation:** Here are a few of the ways to group all the 1's together: 

[0,0,1,1,1,0,0] using 1 swap. 

[0,1,1,1,0,0,0] using 1 swap. 

[1,1,0,0,0,0,1] using 2 swaps (using the circular property of the array). 

There is no way to group all 1's together with 0 swaps. 

Thus, the minimum number of swaps required is 1.

**Example 2:**

**Input:** nums = [0,1,1,1,0,0,1,1,0]

**Output:** 2

**Explanation:** Here are a few of the ways to group all the 1's together: 

[1,1,1,0,0,0,0,1,1] using 2 swaps (using the circular property of the array). 

[1,1,1,1,1,0,0,0,0] using 2 swaps. 

There is no way to group all 1's together with 0 or 1 swaps. 

Thus, the minimum number of swaps required is 2.

**Example 3:**

**Input:** nums = [1,1,0,0,1]

**Output:** 0

**Explanation:** All the 1's are already grouped together due to the circular property of the array. 

Thus, the minimum number of swaps required is 0.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun minSwaps(nums: IntArray): Int {
        val l = nums.size
        val ones = IntArray(l)
        ones[0] = if (nums[0] == 1) 1 else 0
        for (i in 1 until l) {
            if (nums[i] == 1) {
                ones[i] = ones[i - 1] + 1
            } else {
                ones[i] = ones[i - 1]
            }
        }
        if (ones[l - 1] == l || ones[l - 1] == 0) {
            return 0
        }
        val ws = ones[l - 1]
        var minSwaps = Int.MAX_VALUE
        var si = 0
        var ei: Int
        while (si < nums.size) {
            ei = (si + ws - 1) % l
            var totalones: Int
            totalones = if (ei >= si) {
                ones[ei] - if (si == 0) 0 else ones[si - 1]
            } else {
                ones[ei] + (ones[l - 1] - ones[si - 1])
            }
            val swapsreq = ws - totalones
            if (swapsreq < minSwaps) {
                minSwaps = swapsreq
            }
            si++
        }
        return minSwaps
    }
}
```