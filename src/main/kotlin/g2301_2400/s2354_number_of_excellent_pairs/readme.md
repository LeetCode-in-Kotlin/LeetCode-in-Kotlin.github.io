[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2354\. Number of Excellent Pairs

Hard

You are given a **0-indexed** positive integer array `nums` and a positive integer `k`.

A pair of numbers `(num1, num2)` is called **excellent** if the following conditions are satisfied:

*   **Both** the numbers `num1` and `num2` exist in the array `nums`.
*   The sum of the number of set bits in `num1 OR num2` and `num1 AND num2` is greater than or equal to `k`, where `OR` is the bitwise **OR** operation and `AND` is the bitwise **AND** operation.

Return _the number of **distinct** excellent pairs_.

Two pairs `(a, b)` and `(c, d)` are considered distinct if either `a != c` or `b != d`. For example, `(1, 2)` and `(2, 1)` are distinct.

**Note** that a pair `(num1, num2)` such that `num1 == num2` can also be excellent if you have at least **one** occurrence of `num1` in the array.

**Example 1:**

**Input:** nums = [1,2,3,1], k = 3

**Output:** 5

**Explanation:** The excellent pairs are the following:

- (3, 3). (3 AND 3) and (3 OR 3) are both equal to (11) in binary. The total number of set bits is 2 + 2 = 4, which is greater than or equal to k = 3.

- (2, 3) and (3, 2). (2 AND 3) is equal to (10) in binary, and (2 OR 3) is equal to (11) in binary. The total number of set bits is 1 + 2 = 3.

- (1, 3) and (3, 1). (1 AND 3) is equal to (01) in binary, and (1 OR 3) is equal to (11) in binary. The total number of set bits is 1 + 2 = 3.

So the number of excellent pairs is 5.

**Example 2:**

**Input:** nums = [5,1,1], k = 10

**Output:** 0

**Explanation:** There are no excellent pairs for this array. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= 60`

## Solution

```kotlin
class Solution {
    fun countExcellentPairs(nums: IntArray, k: Int): Long {
        val cnt = LongArray(30)
        var res = 0L
        val set: MutableSet<Int> = HashSet()
        for (a in nums) {
            set.add(a)
        }
        for (a in set) {
            cnt[Integer.bitCount(a)]++
        }
        for (i in 1..29) {
            for (j in 1..29) {
                if (i + j >= k) {
                    res += cnt[i] * cnt[j]
                }
            }
        }
        return res
    }
}
```