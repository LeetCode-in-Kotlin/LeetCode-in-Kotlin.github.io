[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2834\. Find the Minimum Possible Sum of a Beautiful Array

Medium

You are given positive integers `n` and `target`.

An array `nums` is **beautiful** if it meets the following conditions:

*   `nums.length == n`.
*   `nums` consists of pairwise **distinct** **positive** integers.
*   There doesn't exist two **distinct** indices, `i` and `j`, in the range `[0, n - 1]`, such that `nums[i] + nums[j] == target`.

Return _the **minimum** possible sum that a beautiful array could have modulo_ <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 2, target = 3

**Output:** 4

**Explanation:** We can see that nums = [1,3] is beautiful. 
- The array nums has length n = 2. 
- The array nums consists of pairwise distinct positive integers. 
- There doesn't exist two distinct indices, i and j, with nums[i] + nums[j] == 3. 

It can be proven that 4 is the minimum possible sum that a beautiful array could have.

**Example 2:**

**Input:** n = 3, target = 3

**Output:** 8

**Explanation:** We can see that nums = [1,3,4] is beautiful. 
- The array nums has length n = 3. 
- The array nums consists of pairwise distinct positive integers. 
- There doesn't exist two distinct indices, i and j, with nums[i] + nums[j] == 3.

It can be proven that 8 is the minimum possible sum that a beautiful array could have.

**Example 3:**

**Input:** n = 1, target = 1

**Output:** 1

**Explanation:** We can see, that nums = [1] is beautiful.

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   <code>1 <= target <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minimumPossibleSum(n: Int, target: Int): Int {
        val mod = 1e9.toLong() + 7
        if (target > (n + n - 1)) {
            return (n.toLong() * (n + 1) % mod / 2).toInt()
        }
        val toChange = n - (target / 2).toLong()
        val sum = ((n * (n.toLong() + 1)) / 2) % mod
        val remain = target.toLong() - ((target / 2) + 1)
        return ((sum + (toChange * remain) % mod) % mod).toInt()
    }
}
```