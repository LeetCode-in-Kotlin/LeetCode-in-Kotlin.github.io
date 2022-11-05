[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 275\. H-Index II

Medium

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their <code>i<sup>th</sup></code> paper and `citations` is sorted in an **ascending order**, return compute the researcher's `h`**\-index**.

According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): A scientist has an index `h` if `h` of their `n` papers have at least `h` citations each, and the other `n − h` papers have no more than `h` citations each.

If there are several possible values for `h`, the maximum one is taken as the `h`**\-index**.

You must write an algorithm that runs in logarithmic time.

**Example 1:**

**Input:** citations = [0,1,3,5,6]

**Output:** 3

**Explanation:** [0,1,3,5,6] means the researcher has 5 papers in total and each of them had received 0, 1, 3, 5, 6 citations respectively. Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.

**Example 2:**

**Input:** citations = [1,2,100]

**Output:** 2

**Constraints:**

*   `n == citations.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= citations[i] <= 1000`
*   `citations` is sorted in **ascending order**.

## Solution

```kotlin
class Solution {
    fun hIndex(citations: IntArray): Int {
        var lo = 1
        var hi = 1000
        var ans = 0
        while (lo <= hi) {
            val mid = (lo + hi) / 2
            val p = check(mid, citations)
            if (citations.size - p >= mid) {
                ans = mid
                lo = mid + 1
            } else {
                hi = mid - 1
            }
        }
        return ans
    }

    private fun check(v: Int, arr: IntArray): Int {
        var lo = 0
        var hi = arr.size - 1
        while (lo <= hi) {
            val mid = (lo + hi) / 2
            if (arr[mid] < v) {
                lo = mid + 1
            } else {
                hi = mid - 1
            }
        }
        return lo
    }
}
```