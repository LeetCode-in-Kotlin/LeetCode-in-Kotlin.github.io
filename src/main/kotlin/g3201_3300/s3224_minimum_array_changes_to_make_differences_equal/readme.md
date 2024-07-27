[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3224\. Minimum Array Changes to Make Differences Equal

Medium

You are given an integer array `nums` of size `n` where `n` is **even**, and an integer `k`.

You can perform some changes on the array, where in one change you can replace **any** element in the array with **any** integer in the range from `0` to `k`.

You need to perform some changes (possibly none) such that the final array satisfies the following condition:

*   There exists an integer `X` such that `abs(a[i] - a[n - i - 1]) = X` for all `(0 <= i < n)`.

Return the **minimum** number of changes required to satisfy the above condition.

**Example 1:**

**Input:** nums = [1,0,1,2,4,3], k = 4

**Output:** 2

**Explanation:**   
 We can perform the following changes:

*   Replace `nums[1]` by 2. The resulting array is <code>nums = [1,<ins>**2**</ins>,1,2,4,3]</code>.
*   Replace `nums[3]` by 3. The resulting array is <code>nums = [1,2,1,<ins>**3**</ins>,4,3]</code>.

The integer `X` will be 2.

**Example 2:**

**Input:** nums = [0,1,2,3,3,6,5,4], k = 6

**Output:** 2

**Explanation:**   
 We can perform the following operations:

*   Replace `nums[3]` by 0. The resulting array is <code>nums = [0,1,2,<ins>**0**</ins>,3,6,5,4]</code>.
*   Replace `nums[4]` by 4. The resulting array is <code>nums = [0,1,2,0,**<ins>4</ins>**,6,5,4]</code>.

The integer `X` will be 4.

**Constraints:**

*   <code>2 <= n == nums.length <= 10<sup>5</sup></code>
*   `n` is even.
*   <code>0 <= nums[i] <= k <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minChanges(nums: IntArray, k: Int): Int {
        val cm = IntArray(k + 2)
        for (i in 0 until nums.size / 2) {
            val a = min(nums[i], nums[nums.size - 1 - i])
            val b = max(nums[i], nums[nums.size - 1 - i])
            val d = b - a
            if (d > 0) {
                cm[0]++
                cm[d]--
                cm[d + 1]++
                val max = (max(a, (k - b)) + d)
                cm[max + 1]++
            } else {
                cm[1]++
                val max = max(a, (k - a))
                cm[max + 1]++
            }
        }
        var sum = cm[0]
        var res = cm[0]
        for (i in 1..k) {
            sum += cm[i]
            res = min(res, sum)
        }
        return res
    }
}
```