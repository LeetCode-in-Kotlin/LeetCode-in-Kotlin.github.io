[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1803\. Count Pairs With XOR in a Range

Hard

Given a **(0-indexed)** integer array `nums` and two integers `low` and `high`, return _the number of **nice pairs**_.

A **nice pair** is a pair `(i, j)` where `0 <= i < j < nums.length` and `low <= (nums[i] XOR nums[j]) <= high`.

**Example 1:**

**Input:** nums = [1,4,2,7], low = 2, high = 6

**Output:** 6

**Explanation:** All nice pairs (i, j) are as follows:

- (0, 1): nums[0] XOR nums[1] = 5 

- (0, 2): nums[0] XOR nums[2] = 3 

- (0, 3): nums[0] XOR nums[3] = 6 

- (1, 2): nums[1] XOR nums[2] = 6 

- (1, 3): nums[1] XOR nums[3] = 3 

- (2, 3): nums[2] XOR nums[3] = 5

**Example 2:**

**Input:** nums = [9,8,4,2,1], low = 5, high = 14

**Output:** 8

**Explanation:** All nice pairs (i, j) are as follows: 

- (0, 2): nums[0] XOR nums[2] = 13 

- (0, 3): nums[0] XOR nums[3] = 11 

- (0, 4): nums[0] XOR nums[4] = 8 

- (1, 2): nums[1] XOR nums[2] = 12 

- (1, 3): nums[1] XOR nums[3] = 10 

- (1, 4): nums[1] XOR nums[4] = 9 

- (2, 3): nums[2] XOR nums[3] = 6 

- (2, 4): nums[2] XOR nums[4] = 5

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 2 * 10<sup>4</sup></code>
*   <code>1 <= low <= high <= 2 * 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun countPairs(nums: IntArray, low: Int, high: Int): Int {
        val root = Trie()
        var pairsCount = 0
        for (num in nums) {
            val pairsCountHigh = countPairsWhoseXorLessThanX(num, root, high + 1)
            val pairsCountLow = countPairsWhoseXorLessThanX(num, root, low)
            pairsCount += pairsCountHigh - pairsCountLow
            root.insertNumber(num)
        }
        return pairsCount
    }

    private fun countPairsWhoseXorLessThanX(num: Int, root: Trie, x: Int): Int {
        var pairs = 0
        var curr: Trie? = root
        var i = 14
        while (i >= 0 && curr != null) {
            val numIthBit = num shr i and 1
            val xIthBit = x shr i and 1
            if (xIthBit == 1) {
                if (curr.child[numIthBit] != null) {
                    pairs += curr.child[numIthBit]!!.count
                }
                curr = curr.child[1 - numIthBit]
            } else {
                curr = curr.child[numIthBit]
            }
            i--
        }
        return pairs
    }

    private class Trie {
        var child: Array<Trie?> = arrayOfNulls(2)
        var count: Int = 0

        fun insertNumber(num: Int) {
            var curr = this
            for (i in 14 downTo 0) {
                val ithBit = num shr i and 1
                if (curr.child[ithBit] == null) {
                    curr.child[ithBit] = Trie()
                }
                curr.child[ithBit]!!.count++
                curr = curr.child[ithBit]!!
            }
        }
    }
}
```