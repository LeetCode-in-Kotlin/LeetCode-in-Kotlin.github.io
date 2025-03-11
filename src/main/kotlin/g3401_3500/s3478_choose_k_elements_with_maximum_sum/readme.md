[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3478\. Choose K Elements With Maximum Sum

Medium

You are given two integer arrays, `nums1` and `nums2`, both of length `n`, along with a positive integer `k`.

For each index `i` from `0` to `n - 1`, perform the following:

*   Find **all** indices `j` where `nums1[j]` is less than `nums1[i]`.
*   Choose **at most** `k` values of `nums2[j]` at these indices to **maximize** the total sum.

Return an array `answer` of size `n`, where `answer[i]` represents the result for the corresponding index `i`.

**Example 1:**

**Input:** nums1 = [4,2,1,5,3], nums2 = [10,20,30,40,50], k = 2

**Output:** [80,30,0,80,50]

**Explanation:**

*   For `i = 0`: Select the 2 largest values from `nums2` at indices `[1, 2, 4]` where `nums1[j] < nums1[0]`, resulting in `50 + 30 = 80`.
*   For `i = 1`: Select the 2 largest values from `nums2` at index `[2]` where `nums1[j] < nums1[1]`, resulting in 30.
*   For `i = 2`: No indices satisfy `nums1[j] < nums1[2]`, resulting in 0.
*   For `i = 3`: Select the 2 largest values from `nums2` at indices `[0, 1, 2, 4]` where `nums1[j] < nums1[3]`, resulting in `50 + 30 = 80`.
*   For `i = 4`: Select the 2 largest values from `nums2` at indices `[1, 2]` where `nums1[j] < nums1[4]`, resulting in `30 + 20 = 50`.

**Example 2:**

**Input:** nums1 = [2,2,2,2], nums2 = [3,1,2,3], k = 1

**Output:** [0,0,0,0]

**Explanation:**

Since all elements in `nums1` are equal, no indices satisfy the condition `nums1[j] < nums1[i]` for any `i`, resulting in 0 for all positions.

**Constraints:**

*   `n == nums1.length == nums2.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>6</sup></code>
*   `1 <= k <= n`

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun findMaxSum(nums1: IntArray, nums2: IntArray, k: Int): LongArray {
        val n = nums1.size
        val ans = LongArray(n)
        val ps = Array<Point>(n) { i -> Point(nums1[i], nums2[i], i) }
        ps.sortWith { p1: Point, p2: Point -> p1.x.compareTo(p2.x) }
        val pq = PriorityQueue<Int?>()
        var s: Long = 0
        var i = 0
        while (i < n) {
            var j = i
            while (j < n && ps[j].x == ps[i].x) {
                ans[ps[j].i] = s
                j++
            }
            for (p in i..<j) {
                val cur = ps[p].y
                if (pq.size < k) {
                    pq.offer(cur)
                    s += cur.toLong()
                } else if (cur > pq.peek()!!) {
                    s -= pq.poll()!!.toLong()
                    pq.offer(cur)
                    s += cur.toLong()
                }
            }
            i = j
        }
        return ans
    }

    private class Point(var x: Int, var y: Int, var i: Int)
}
```