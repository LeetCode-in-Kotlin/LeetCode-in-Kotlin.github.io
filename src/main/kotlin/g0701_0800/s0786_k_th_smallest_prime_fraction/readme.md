[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 786\. K-th Smallest Prime Fraction

Medium

You are given a sorted integer array `arr` containing `1` and **prime** numbers, where all the integers of `arr` are unique. You are also given an integer `k`.

For every `i` and `j` where `0 <= i < j < arr.length`, we consider the fraction `arr[i] / arr[j]`.

Return _the_ <code>k<sup>th</sup></code> _smallest fraction considered_. Return your answer as an array of integers of size `2`, where `answer[0] == arr[i]` and `answer[1] == arr[j]`.

**Example 1:**

**Input:** arr = [1,2,3,5], k = 3

**Output:** [2,5]

**Explanation:** The fractions to be considered in sorted order are: 1/5, 1/3, 2/5, 1/2, 3/5, and 2/3. The third fraction is 2/5.

**Example 2:**

**Input:** arr = [1,7], k = 1

**Output:** [1,7]

**Constraints:**

*   `2 <= arr.length <= 1000`
*   <code>1 <= arr[i] <= 3 * 10<sup>4</sup></code>
*   `arr[0] == 1`
*   `arr[i]` is a **prime** number for `i > 0`.
*   All the numbers of `arr` are **unique** and sorted in **strictly increasing** order.
*   `1 <= k <= arr.length * (arr.length - 1) / 2`

**Follow up:** Can you solve the problem with better than <code>O(n<sup>2</sup>)</code> complexity?

## Solution

```kotlin
class Solution {
    fun kthSmallestPrimeFraction(arr: IntArray, k: Int): IntArray {
        val n = arr.size
        var low = 0.0
        var high = 1.0
        while (low < high) {
            val mid = (low + high) / 2
            val res = getFractionsLessThanMid(arr, n, mid)
            if (res[0] == k) {
                return intArrayOf(arr[res[1]], arr[res[2]])
            } else if (res[0] > k) {
                high = mid
            } else {
                low = mid
            }
        }
        return intArrayOf()
    }

    private fun getFractionsLessThanMid(arr: IntArray, n: Int, mid: Double): IntArray {
        var maxLessThanMid = 0.0
        // stores indices of max fraction less than mid;
        var x = 0
        var y = 0
        // for storing fractions less than mid
        var total = 0
        var j = 1
        for (i in 0 until n - 1) {
            // while fraction is greater than mid increment j
            while (j < n && arr[i] > arr[j] * mid) {
                j++
            }
            if (j == n) {
                break
            }
            // j fractions greater than mid, n-j fractions smaller than mid, add fractions smaller
            // than mid to total
            total += n - j
            val fraction = arr[i].toDouble() / arr[j]
            if (fraction > maxLessThanMid) {
                maxLessThanMid = fraction
                x = i
                y = j
            }
        }
        return intArrayOf(total, x, y)
    }
}
```