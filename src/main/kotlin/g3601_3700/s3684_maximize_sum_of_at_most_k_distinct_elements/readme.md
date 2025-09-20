[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3684\. Maximize Sum of At Most K Distinct Elements

Easy

You are given a **positive** integer array `nums` and an integer `k`.

Choose at most `k` elements from `nums` so that their sum is maximized. However, the chosen numbers must be **distinct**.

Return an array containing the chosen numbers in **strictly descending** order.

**Example 1:**

**Input:** nums = [84,93,100,77,90], k = 3

**Output:** [100,93,90]

**Explanation:**

The maximum sum is 283, which is attained by choosing 93, 100 and 90. We rearrange them in strictly descending order as `[100, 93, 90]`.

**Example 2:**

**Input:** nums = [84,93,100,77,93], k = 3

**Output:** [100,93,84]

**Explanation:**

The maximum sum is 277, which is attained by choosing 84, 93 and 100. We rearrange them in strictly descending order as `[100, 93, 84]`. We cannot choose 93, 100 and 93 because the chosen numbers must be distinct.

**Example 3:**

**Input:** nums = [1,1,1,2,2,2], k = 6

**Output:** [2,1]

**Explanation:**

The maximum sum is 3, which is attained by choosing 1 and 2. We rearrange them in strictly descending order as `[2, 1]`.

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= nums.length`

## Solution

```kotlin
class Solution {
    fun maxKDistinct(nums: IntArray, k: Int): IntArray {
        nums.sort()
        val arr = IntArray(k)
        var j = 1
        arr[0] = nums[nums.size - 1]
        if (nums.size > 1) {
            var i = nums.size - 2
            while (j < k && i >= 0) {
                if (i < nums.size - 1 && nums[i] != nums[i + 1]) {
                    arr[j] = nums[i]
                    j++
                }
                i--
            }
        }
        var cnt = 0
        var n = 0
        while (n < arr.size) {
            if (arr[n] != 0) {
                cnt++
            }
            n++
        }
        val finl = IntArray(cnt)
        System.arraycopy(arr, 0, finl, 0, cnt)
        return finl
    }
}
```