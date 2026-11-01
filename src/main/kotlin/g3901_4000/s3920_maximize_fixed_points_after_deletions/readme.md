[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3920\. Maximize Fixed Points After Deletions

Hard

You are given an integer array `nums`.

A position `i` is called a **fixed point** if `nums[i] == i`.

You are allowed to delete **any** number of elements (including zero) from the array. After each deletion, the remaining elements **shift left**, and indices are reassigned starting from 0.

Return an integer denoting the **maximum** number of fixed points that can be achieved after performing any number of deletions.

**Example 1:**

**Input:** nums = [0,2,1]

**Output:** 2

**Explanation:**

*   Delete `nums[1] = 2`. The array becomes `[0, 1]`.
*   Now, `nums[0] = 0` and `nums[1] = 1`, so both indices are fixed points.
*   Thus, the answer is 2.

**Example 2:**

**Input:** nums = [3,1,2]

**Output:** 2

**Explanation:**

*   Do not delete any elements. The array remains `[3, 1, 2]`.
*   Here, `nums[1] = 1` and `nums[2] = 2`, so these indices are fixed points.
*   Thus, the answer is 2.

**Example 3:**

**Input:** nums = [1,0,1,2]

**Output:** 3

**Explanation:**

*   Delete `nums[0] = 1`. The array becomes `[0, 1, 2]`.
*   Now, `nums[0] = 0`, `nums[1] = 1`, and `nums[2] = 2`, so all indices are fixed points.
*   Thus, the answer is 3.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun maxFixedPoints(nums: IntArray): Int {
        val n = nums.size
        val arr = LongArray(n)
        var index = 0
        for (i in 0..<n) {
            if (nums[i] <= i) {
                arr[index++] = i.toLong() - nums[i] shl 32 or nums[i].toLong()
            }
        }
        if (index == 0) {
            return 0
        }
        arr.sort(0, index)
        var max = 0
        val lis = IntArray(index)
        lis[0] = arr[0].toInt()
        for (i in 1..<index) {
            val `val` = arr[i].toInt()
            lis[if (`val` > lis[max]) ++max else binarySearch(lis, `val`, max)] = `val`
        }
        return max + 1
    }

    private fun binarySearch(arr: IntArray, target: Int, right: Int): Int {
        var right = right
        var left = 0
        while (left < right) {
            val mid = left + right ushr 1
            if (arr[mid] >= target) {
                right = mid
            } else {
                left = mid + 1
            }
        }
        return left
    }
}
```