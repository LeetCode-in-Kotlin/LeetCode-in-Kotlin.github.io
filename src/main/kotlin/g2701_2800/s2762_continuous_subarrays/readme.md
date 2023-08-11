[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2762\. Continuous Subarrays

Medium

You are given a **0-indexed** integer array `nums`. A subarray of `nums` is called **continuous** if:

*   Let `i`, `i + 1`, ..., `j` be the indices in the subarray. Then, for each pair of indices <code>i <= i<sub>1</sub>, i<sub>2</sub> <= j</code>, <code>0 <= |nums[i<sub>1</sub>] - nums[i<sub>2</sub>]| <= 2</code>.

Return _the total number of **continuous** subarrays._

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [5,4,2,4]

**Output:** 8

**Explanation:** 

Continuous subarray of size 1: [5], [4], [2], [4]. 

Continuous subarray of size 2: [5,4], [4,2], [2,4]. 

Continuous subarray of size 3: [4,2,4]. 

Thereare no subarrys of size 4. 

Total continuous subarrays = 4 + 3 + 1 = 8. 

It can be shown that there are no more continuous subarrays.

**Example 2:**

**Input:** nums = [1,2,3]

**Output:** 6

**Explanation:** 

Continuous subarray of size 1: [1], [2], [3]. 

Continuous subarray of size 2: [1,2], [2,3]. 

Continuous subarray of size 3: [1,2,3]. 

Total continuous subarrays = 3 + 2 + 1 = 6.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.TreeMap

class Solution {
    fun continuousSubarrays(nums: IntArray): Long {
        var left = 0
        var right = 0
        var total = 0L
        val tree = TreeMap<Int, Int>()
        val n = nums.size
        while (right < n) {
            if (!tree.containsKey(nums[right])) {
                tree[nums[right]] = 0
            }
            tree[nums[right]] = tree[nums[right]]!! + 1
            while (kotlin.math.abs(tree.lastKey() - nums[right]) > 2 || Math.abs(tree.firstKey() - nums[right]) > 2) {
                val keyL = nums[left]
                tree[keyL] = tree[keyL]!! - 1
                if (tree[keyL] == 0) tree.remove(keyL)
                left++
            }
            total += right - left + 1
            right++
        }
        return total
    }
}
```