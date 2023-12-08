[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2813\. Maximum Elegance of a K-Length Subsequence

Hard

You are given a **0-indexed** 2D integer array `items` of length `n` and an integer `k`.

<code>items[i] = [profit<sub>i</sub>, category<sub>i</sub>]</code>, where <code>profit<sub>i</sub></code> and <code>category<sub>i</sub></code> denote the profit and category of the <code>i<sup>th</sup></code> item respectively.

Let's define the **elegance** of a **subsequence** of `items` as <code>total_profit + distinct_categories<sup>2</sup></code>, where `total_profit` is the sum of all profits in the subsequence, and `distinct_categories` is the number of **distinct** categories from all the categories in the selected subsequence.

Your task is to find the **maximum elegance** from all subsequences of size `k` in `items`.

Return _an integer denoting the maximum elegance of a subsequence of_ `items` _with size exactly_ `k`.

**Note:** A subsequence of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order.

**Example 1:**

**Input:** items = \[\[3,2],[5,1],[10,1]], k = 2

**Output:** 17

**Explanation:**

In this example, we have to select a subsequence of size 2.

We can select items[0] = [3,2] and items[2] = [10,1].

The total profit in this subsequence is 3 + 10 = 13, and the subsequence contains 2 distinct categories [2,1].

Hence, the elegance is 13 + 2<sup>2</sup> = 17, and we can show that it is the maximum achievable elegance. 

**Example 2:**

**Input:** items = \[\[3,1],[3,1],[2,2],[5,3]], k = 3

**Output:** 19

**Explanation:**

In this example, we have to select a subsequence of size 3.

We can select items[0] = [3,1], items[2] = [2,2], and items[3] = [5,3].

The total profit in this subsequence is 3 + 2 + 5 = 10, and the subsequence contains 3 distinct categories [1,2,3].

Hence, the elegance is 10 + 3<sup>2</sup> = 19, and we can show that it is the maximum achievable elegance.

**Example 3:**

**Input:** items = \[\[1,1],[2,1],[3,1]], k = 3

**Output:** 7

**Explanation:**

In this example, we have to select a subsequence of size 3.

We should select all the items.

The total profit will be 1 + 2 + 3 = 6, and the subsequence contains 1 distinct category [1].

Hence, the maximum elegance is 6 + 1<sup>2</sup> = 7. 

**Constraints:**

*   <code>1 <= items.length == n <= 10<sup>5</sup></code>
*   `items[i].length == 2`
*   <code>items[i][0] == profit<sub>i</sub></code>
*   <code>items[i][1] == category<sub>i</sub></code>
*   <code>1 <= profit<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= category<sub>i</sub> <= n</code>
*   `1 <= k <= n`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun findMaximumElegance(items: Array<IntArray>, k: Int): Long {
        items.sortWith { a: IntArray, b: IntArray -> b[0] - a[0] }
        val n = items.size
        val vis = BooleanArray(n)
        val arr = ArrayDeque<Long>()
        var distinct: Long = 0
        var sum: Long = 0
        for (i in 0 until k) {
            sum += items[i][0].toLong()
            if (vis[items[i][1] - 1]) {
                arr.addLast(items[i][0].toLong())
            } else {
                ++distinct
                vis[items[i][1] - 1] = true
            }
        }
        var ans = sum + distinct * distinct
        var i = k
        while (i < n && distinct < k) {
            if (!vis[items[i][1] - 1]) {
                sum -= arr.removeLast()
                sum += items[i][0].toLong()
                ++distinct
                vis[items[i][1] - 1] = true
                ans = max(ans.toDouble(), (sum + distinct * distinct).toDouble()).toLong()
            }
            i++
        }
        return ans
    }
}
```