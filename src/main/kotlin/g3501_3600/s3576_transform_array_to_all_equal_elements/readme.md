[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3576\. Transform Array to All Equal Elements

Medium

You are given an integer array `nums` of size `n` containing only `1` and `-1`, and an integer `k`.

You can perform the following operation at most `k` times:

*   Choose an index `i` (`0 <= i < n - 1`), and **multiply** both `nums[i]` and `nums[i + 1]` by `-1`.
    

**Note** that you can choose the same index `i` more than once in **different** operations.

Return `true` if it is possible to make all elements of the array **equal** after at most `k` operations, and `false` otherwise.

**Example 1:**

**Input:** nums = [1,-1,1,-1,1], k = 3

**Output:** true

**Explanation:**

We can make all elements in the array equal in 2 operations as follows:

*   Choose index `i = 1`, and multiply both `nums[1]` and `nums[2]` by -1. Now `nums = [1,1,-1,-1,1]`.
*   Choose index `i = 2`, and multiply both `nums[2]` and `nums[3]` by -1. Now `nums = [1,1,1,1,1]`.

**Example 2:**

**Input:** nums = [-1,-1,-1,1,1,1], k = 5

**Output:** false

**Explanation:**

It is not possible to make all array elements equal in at most 5 operations.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either -1 or 1.
*   `1 <= k <= n`

## Solution

```kotlin
class Solution {
    fun canMakeEqual(nums: IntArray, k: Int): Boolean {
        val n = nums.size
        if (n == 1) {
            return true
        }
        var prod = 1
        for (x in nums) {
            prod *= x
        }
        val targets: MutableList<Int> = ArrayList<Int>()
        for (target in intArrayOf(1, -1)) {
            val tPowN = (if (n % 2 == 0) 1 else target)
            if (tPowN == prod) {
                targets.add(target)
            }
        }
        if (targets.isEmpty()) {
            return false
        }
        for (target in targets) {
            var ops = 0
            val a = nums.clone()
            var i = 0
            while (i < n - 1 && ops <= k) {
                if (a[i] != target) {
                    a[i] = -a[i]
                    a[i + 1] = -a[i + 1]
                    ops++
                }
                i++
            }
            if (ops <= k && a[n - 1] == target) {
                return true
            }
        }
        return false
    }
}
```