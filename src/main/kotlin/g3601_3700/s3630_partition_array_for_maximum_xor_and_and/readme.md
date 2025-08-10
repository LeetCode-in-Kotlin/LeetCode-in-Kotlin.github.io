[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3630\. Partition Array for Maximum XOR and AND

Hard

You are given an integer array `nums`.

Partition the array into **three** (possibly empty) **subsequences** `A`, `B`, and `C` such that every element of `nums` belongs to **exactly** one subsequence.

Your goal is to **maximize** the value of: `XOR(A) + AND(B) + XOR(C)`

where:

*   `XOR(arr)` denotes the bitwise XOR of all elements in `arr`. If `arr` is empty, its value is defined as 0.
*   `AND(arr)` denotes the bitwise AND of all elements in `arr`. If `arr` is empty, its value is defined as 0.

Return the **maximum** value achievable.

**Note:** If multiple partitions result in the same **maximum** sum, you can consider any one of them.

**Example 1:**

**Input:** nums = [2,3]

**Output:** 5

**Explanation:**

One optimal partition is:

*   `A = [3], XOR(A) = 3`
*   `B = [2], AND(B) = 2`
*   `C = [], XOR(C) = 0`

The maximum value of: `XOR(A) + AND(B) + XOR(C) = 3 + 2 + 0 = 5`. Thus, the answer is 5.

**Example 2:**

**Input:** nums = [1,3,2]

**Output:** 6

**Explanation:**

One optimal partition is:

*   `A = [1], XOR(A) = 1`
*   `B = [2], AND(B) = 2`
*   `C = [3], XOR(C) = 3`

The maximum value of: `XOR(A) + AND(B) + XOR(C) = 1 + 2 + 3 = 6`. Thus, the answer is 6.

**Example 3:**

**Input:** nums = [2,3,6,7]

**Output:** 15

**Explanation:**

One optimal partition is:

*   `A = [7], XOR(A) = 7`
*   `B = [2,3], AND(B) = 2`
*   `C = [6], XOR(C) = 6`

The maximum value of: `XOR(A) + AND(B) + XOR(C) = 7 + 2 + 6 = 15`. Thus, the answer is 15.

**Constraints:**

*   `1 <= nums.length <= 19`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun maximizeXorAndXor(nums: IntArray): Long {
        val n = nums.size
        val full = 1 shl n
        val xorMask = IntArray(full)
        val andMask = IntArray(full)
        val orMask = IntArray(full)
        for (mask in 1..<full) {
            val lb = mask and -mask
            val i = Integer.numberOfTrailingZeros(lb)
            val prev = mask xor lb
            xorMask[mask] = xorMask[prev] xor nums[i]
            andMask[mask] = if (prev == 0) nums[i] else andMask[prev] and nums[i]
            orMask[mask] = orMask[prev] or nums[i]
        }
        var best: Long = 0
        val all = full - 1
        for (b in 0..<full) {
            val andB = andMask[b].toLong()
            val rest = all xor b
            if (andB + 2L * orMask[rest] <= best) {
                continue
            }
            var a = rest
            while (true) {
                val c = rest xor a
                val sum = xorMask[a] + andB + xorMask[c]
                if (sum > best) {
                    best = sum
                }
                if (a == 0) {
                    break
                }
                a = (a - 1) and rest
            }
        }
        return best
    }
}
```