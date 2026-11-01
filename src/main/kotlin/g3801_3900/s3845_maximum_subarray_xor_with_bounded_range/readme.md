[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3845\. Maximum Subarray XOR with Bounded Range

Hard

You are given a non-negative integer array `nums` and an integer `k`.

You must select a **non-empty subarrays** of `nums` such that the **difference** between its **maximum** and **minimum** elements is at most `k`. The **value** of this subarray is the bitwise XOR of all elements in the subarray.

Return an integer denoting the **maximum** possible **value** of the selected subarray.

**Example 1:**

**Input:** nums = [5,4,5,6], k = 2

**Output:** 7

**Explanation:**

*   Select the subarray <code>[5, <ins>**4, 5, 6**</ins>]</code>.
*   The difference between its maximum and minimum elements is `6 - 4 = 2 <= k`.
*   The value is `4 XOR 5 XOR 6 = 7`.

**Example 2:**

**Input:** nums = [5,4,5,6], k = 1

**Output:** 6

**Explanation:**

*   Select the subarray <code>[5, 4, 5, <ins>**6**</ins>]</code>.
*   The difference between its maximum and minimum elements is `6 - 6 = 0 <= k`.
*   The value is 6.

**Constraints:**

*   <code>1 <= nums.length <= 4 * 10<sup>4</sup></code>
*   <code>0 <= nums[i] < 2<sup>15</sup></code>
*   <code>0 <= k < 2<sup>15</sup></code>

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque
import kotlin.math.max

class Solution {
    companion object {
        private const val BITS = 15
    }

    class TrieNode {
        val next = arrayOfNulls<TrieNode>(2)
        var count = 0
    }

    private fun insert(root: TrieNode, num: Int) {
        var node = root
        for (i in BITS - 1 downTo 0) {
            val bit = (num ushr i) and 1
            if (node.next[bit] == null) {
                node.next[bit] = TrieNode()
            }
            node = node.next[bit]!!
            node.count++
        }
    }

    private fun remove(root: TrieNode, num: Int) {
        var node = root
        for (i in BITS - 1 downTo 0) {
            val bit = (num ushr i) and 1
            val child = node.next[bit]!!
            child.count--
            if (child.count == 0) {
                node.next[bit] = null
            }
            node = child
        }
    }

    private fun query(root: TrieNode, num: Int): Int {
        var node = root
        var res = 0
        for (i in BITS - 1 downTo 0) {
            val bit = (num ushr i) and 1
            val targetBit = bit xor 1
            if (node.next[targetBit] != null) {
                res = res or (1 shl i)
                node = node.next[targetBit]!!
            } else {
                node = node.next[bit]!!
            }
        }
        return res
    }

    fun maxXor(nums: IntArray, k: Int): Int {
        val n = nums.size
        val prefix = IntArray(n + 1)
        for (i in 0 until n) {
            prefix[i + 1] = prefix[i] xor nums[i]
        }
        val maxDeque: Deque<Int> = ArrayDeque()
        val minDeque: Deque<Int> = ArrayDeque()
        val root = TrieNode()
        var maxXor = 0
        var l = 0
        insert(root, 0)
        for (r in 0 until n) {
            // maintain max deque
            while (maxDeque.isNotEmpty() && nums[r] > nums[maxDeque.peekLast()]) {
                maxDeque.pollLast()
            }
            maxDeque.offerLast(r)
            // maintain min deque
            while (minDeque.isNotEmpty() && nums[r] < nums[minDeque.peekLast()]) {
                minDeque.pollLast()
            }
            minDeque.offerLast(r)
            // shrink window if max - min > k
            while (maxDeque.isNotEmpty() &&
                minDeque.isNotEmpty() &&
                nums[maxDeque.peekFirst()] - nums[minDeque.peekFirst()] > k
            ) {
                remove(root, prefix[l])
                l++
                if (maxDeque.peekFirst() < l) {
                    maxDeque.pollFirst()
                }
                if (minDeque.peekFirst() < l) {
                    minDeque.pollFirst()
                }
            }
            // query and insert prefix[r + 1]
            maxXor = max(maxXor, query(root, prefix[r + 1]))
            insert(root, prefix[r + 1])
        }
        return maxXor
    }
}
```