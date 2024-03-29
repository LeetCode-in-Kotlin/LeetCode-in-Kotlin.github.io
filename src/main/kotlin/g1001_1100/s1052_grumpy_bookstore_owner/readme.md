[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1052\. Grumpy Bookstore Owner

Medium

There is a bookstore owner that has a store open for `n` minutes. Every minute, some number of customers enter the store. You are given an integer array `customers` of length `n` where `customers[i]` is the number of the customer that enters the store at the start of the <code>i<sup>th</sup></code> minute and all those customers leave after the end of that minute.

On some minutes, the bookstore owner is grumpy. You are given a binary array grumpy where `grumpy[i]` is `1` if the bookstore owner is grumpy during the <code>i<sup>th</sup></code> minute, and is `0` otherwise.

When the bookstore owner is grumpy, the customers of that minute are not satisfied, otherwise, they are satisfied.

The bookstore owner knows a secret technique to keep themselves not grumpy for `minutes` consecutive minutes, but can only use it once.

Return _the maximum number of customers that can be satisfied throughout the day_.

**Example 1:**

**Input:** customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3

**Output:** 16

**Explanation:** The bookstore owner keeps themselves not grumpy for the last 3 minutes. The maximum number of customers that can be satisfied = 1 + 1 + 1 + 1 + 7 + 5 = 16.

**Example 2:**

**Input:** customers = [1], grumpy = [0], minutes = 1

**Output:** 1

**Constraints:**

*   `n == customers.length == grumpy.length`
*   <code>1 <= minutes <= n <= 2 * 10<sup>4</sup></code>
*   `0 <= customers[i] <= 1000`
*   `grumpy[i]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun maxSatisfied(customers: IntArray, grumpy: IntArray, minutes: Int): Int {
        // storing numbers of customers who faced grumpy owner till ith minute.
        val grumpySum = IntArray(grumpy.size)
        var ans = 0
        if (grumpy[0] == 1) {
            grumpySum[0] = customers[0]
        } else {
            ans += customers[0]
        }
        for (i in 1 until grumpy.size) {
            if (grumpy[i] == 1) {
                grumpySum[i] = grumpySum[i - 1] + customers[i]
            } else {
                grumpySum[i] = grumpySum[i - 1]
                ans += customers[i]
            }
        }
        // calculating max number of customers who faced grumpy owner in a window of size 'minutes'.
        var max = 0
        for (i in 0..customers.size - minutes) {
            max = if (i == 0) {
                max.coerceAtLeast(grumpySum[i + minutes - 1])
            } else {
                max.coerceAtLeast(grumpySum[i + minutes - 1] - grumpySum[i - 1])
            }
        }
        // making the owner non-grumpy in that max window and adding the number of customers who do
        // not face the grumpy customers.
        ans += max
        return ans
    }
}
```