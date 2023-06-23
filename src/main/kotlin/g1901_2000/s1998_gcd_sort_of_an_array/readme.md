[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1998\. GCD Sort of an Array

Hard

You are given an integer array `nums`, and you can perform the following operation **any** number of times on `nums`:

*   Swap the positions of two elements `nums[i]` and `nums[j]` if `gcd(nums[i], nums[j]) > 1` where `gcd(nums[i], nums[j])` is the **greatest common divisor** of `nums[i]` and `nums[j]`.

Return `true` _if it is possible to sort_ `nums` _in **non-decreasing** order using the above swap method, or_ `false` _otherwise._

**Example 1:**

**Input:** nums = [7,21,3]

**Output:** true

**Explanation:** We can sort [7,21,3] by performing the following operations:

- Swap 7 and 21 because gcd(7,21) = 7. nums = [**21**,**7**,3]

- Swap 21 and 3 because gcd(21,3) = 3. nums = [**3**,7,**21**] 

**Example 2:**

**Input:** nums = [5,2,6,2]

**Output:** false

**Explanation:** It is impossible to sort the array because 5 cannot be swapped with any other element. 

**Example 3:**

**Input:** nums = [10,5,9,3,15]

**Output:** true We can sort [10,5,9,3,15] by performing the following operations:

- Swap 10 and 15 because gcd(10,15) = 5. nums = [**15**,5,9,3,**10**]

- Swap 15 and 3 because gcd(15,3) = 3. nums = [**3**,5,9,**15**,10]

- Swap 10 and 15 because gcd(10,15) = 5. nums = [3,5,9,**10**,**15**] 

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>2 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun gcdSort(nums: IntArray): Boolean {
        val sorted = nums.clone()
        sorted.sort()
        val len = nums.size
        val max = sorted[len - 1]
        // grouping tree child(index)->parent(value), index==value is root
        val nodes = IntArray(max + 1)
        for (j in nums) {
            nodes[j] = -1
        }
        // value: <=0 not sieved, <0 leaf node, 0 or 1 not in nums, >1 grouped
        for (p in 2..max / 2) {
            if (nodes[p] > 0) {
                // sieved so not a prime number.
                continue
            }
            // p is now a prime number, set self as root.
            nodes[p] = p
            var group = p
            var num = p + p
            while (num <= max) {
                var existing = nodes[num]
                if (existing < 0) {
                    // 1st hit, set group
                    nodes[num] = group
                } else if (existing <= 1) {
                    // value doesn't exist in nums
                    nodes[num] = 1
                } else if (root(nodes, existing).also { existing = it } < group) {
                    nodes[group] = existing
                    group = existing
                } else {
                    nodes[existing] = group
                }
                num += p
            }
        }
        for (i in 0 until len) {
            if (root(nodes, nums[i]) != root(nodes, sorted[i])) {
                return false
            }
        }
        return true
    }

    companion object {
        private fun root(nodes: IntArray, num: Int): Int {
            var num = num
            var group: Int
            while (nodes[num].also { group = it } > 0 && group != num) {
                num = group
            }
            return num
        }
    }
}
```