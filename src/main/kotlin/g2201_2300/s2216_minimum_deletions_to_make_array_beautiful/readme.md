[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2216\. Minimum Deletions to Make Array Beautiful

Medium

You are given a **0-indexed** integer array `nums`. The array `nums` is **beautiful** if:

*   `nums.length` is even.
*   `nums[i] != nums[i + 1]` for all `i % 2 == 0`.

Note that an empty array is considered beautiful.

You can delete any number of elements from `nums`. When you delete an element, all the elements to the right of the deleted element will be **shifted one unit to the left** to fill the gap created and all the elements to the left of the deleted element will remain **unchanged**.

Return _the **minimum** number of elements to delete from_ `nums` _to make it_ _beautiful._

**Example 1:**

**Input:** nums = [1,1,2,3,5]

**Output:** 1

**Explanation:** You can delete either `nums[0]` or `nums[1]` to make `nums` = [1,2,3,5] which is beautiful. It can be proven you need at least 1 deletion to make `nums` beautiful.

**Example 2:**

**Input:** nums = [1,1,2,2,3,3]

**Output:** 2

**Explanation:** You can delete `nums[0]` and `nums[5]` to make nums = [1,2,2,3] which is beautiful. It can be proven you need at least 2 deletions to make nums beautiful.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun minDeletion(nums: IntArray): Int {
        var offset = 0
        var res = 0
        var i = 0
        while (i < nums.size) {
            var j = i
            while (j < nums.size - 1 && nums[j + 1] == nums[j] && (j - offset) % 2 == 0) {
                offset++
                res++
                j++
            }
            i = j + 2
        }
        if ((nums.size - offset) % 2 != 0) {
            res++
        }
        return res
    }
}
```