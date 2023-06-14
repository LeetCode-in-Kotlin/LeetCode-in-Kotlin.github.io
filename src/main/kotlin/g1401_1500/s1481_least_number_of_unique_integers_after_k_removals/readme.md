[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1481\. Least Number of Unique Integers after K Removals

Medium

Given an array of integers `arr` and an integer `k`. Find the _least number of unique integers_ after removing **exactly** `k` elements**.**

**Example 1:**

**Input:** arr = [5,5,4], k = 1

**Output:** 1

**Explanation:** Remove the single 4, only 5 is left.

**Example 2:**

**Input:** arr = [4,3,1,1,3,3,2], k = 3

**Output:** 2

**Explanation:** Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.

**Constraints:**

*   `1 <= arr.length <= 10^5`
*   `1 <= arr[i] <= 10^9`
*   `0 <= k <= arr.length`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun findLeastNumOfUniqueInts(arr: IntArray, k: Int): Int {
        var k = k
        val count: MutableMap<Int, Int> = HashMap()
        for (a in arr) {
            count[a] = 1 + count.getOrDefault(a, 0)
        }
        var remaining = count.size
        var occur = 1
        val occurrenceCount = IntArray(arr.size + 1)
        for (v in count.values) {
            ++occurrenceCount[v]
        }
        while (k > 0) {
            if (k - occur * occurrenceCount[occur] >= 0) {
                k -= occur * occurrenceCount[occur]
                remaining -= occurrenceCount[occur++]
            } else {
                return remaining - k / occur
            }
        }
        return remaining
    }
}
```