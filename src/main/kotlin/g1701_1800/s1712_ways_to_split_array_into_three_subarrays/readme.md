[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1712\. Ways to Split Array Into Three Subarrays

Medium

A split of an integer array is **good** if:

*   The array is split into three **non-empty** contiguous subarrays - named `left`, `mid`, `right` respectively from left to right.
*   The sum of the elements in `left` is less than or equal to the sum of the elements in `mid`, and the sum of the elements in `mid` is less than or equal to the sum of the elements in `right`.

Given `nums`, an array of **non-negative** integers, return _the number of **good** ways to split_ `nums`. As the number may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,1,1]

**Output:** 1

**Explanation:** The only good way to split nums is [1] [1] [1].

**Example 2:**

**Input:** nums = [1,2,2,2,5,0]

**Output:** 3

**Explanation:** There are three good ways of splitting nums:

[1] [2] [2,2,5,0] 

[1] [2,2] [2,5,0] 

[1,2] [2,2] [5,0]

**Example 3:**

**Input:** nums = [3,2,1]

**Output:** 0

**Explanation:** There is no good way to split nums.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun waysToSplit(nums: IntArray): Int {
        var sum = 0
        for (num in nums) {
            sum += num
        }
        var cur = 0
        var res: Long = 0
        var i = 0
        var idx1 = 1
        var sum1 = nums[0]
        var idx2 = 1
        var sum2 = nums[0]
        while (i < nums.size) {
            cur += nums[i]
            val right = sum - cur
            if (i == 0 || i == nums.size - 1) {
                i++
                continue
            }
            while (idx1 <= i && sum1 <= cur - sum1) {
                sum1 += nums[idx1++]
            }
            while (idx2 < idx1 && cur - sum2 > right) {
                sum2 += nums[idx2++]
            }
            if (idx1 > idx2) {
                res = (res + idx1 - idx2) % 1000000007
            }
            i++
        }
        return res.toInt()
    }
}
```