[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1815\. Maximum Number of Groups Getting Fresh Donuts

Hard

There is a donuts shop that bakes donuts in batches of `batchSize`. They have a rule where they must serve **all** of the donuts of a batch before serving any donuts of the next batch. You are given an integer `batchSize` and an integer array `groups`, where `groups[i]` denotes that there is a group of `groups[i]` customers that will visit the shop. Each customer will get exactly one donut.

When a group visits the shop, all customers of the group must be served before serving any of the following groups. A group will be happy if they all get fresh donuts. That is, the first customer of the group does not receive a donut that was left over from the previous group.

You can freely rearrange the ordering of the groups. Return _the **maximum** possible number of happy groups after rearranging the groups._

**Example 1:**

**Input:** batchSize = 3, groups = [1,2,3,4,5,6]

**Output:** 4

**Explanation:** You can arrange the groups as [6,2,4,5,1,3]. Then the 1<sup>st</sup>, 2<sup>nd</sup>, 4<sup>th</sup>, and 6<sup>th</sup> groups will be happy.

**Example 2:**

**Input:** batchSize = 4, groups = [1,3,2,5,2,2,1,6]

**Output:** 4

**Constraints:**

*   `1 <= batchSize <= 9`
*   `1 <= groups.length <= 30`
*   <code>1 <= groups[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Objects

class Solution {
    inner class Data(var idx: Int, var arrHash: Int) {
        override fun equals(other: Any?): Boolean {
            if (this === other) {
                return true
            }
            if (other == null || javaClass != other.javaClass) {
                return false
            }
            val data = other as Data
            return idx == data.idx && arrHash == data.arrHash
        }

        override fun hashCode(): Int {
            return Objects.hash(idx, arrHash)
        }
    }

    private var dp: HashMap<Data, Int> = HashMap()
    fun maxHappyGroups(batchSize: Int, groups: IntArray): Int {
        val arr = IntArray(batchSize)
        for (group in groups) {
            arr[group % batchSize]++
        }
        return arr[0] + solve(0, arr)
    }

    private fun solve(num: Int, arr: IntArray): Int {
        if (isFull(arr)) {
            return 0
        }
        val key = Data(num, arr.contentHashCode())
        if (dp.containsKey(key)) {
            return dp[key]!!
        }
        var best = Int.MIN_VALUE / 2
        if (num == 0) {
            for (i in 1 until arr.size) {
                if (arr[i] <= 0) {
                    continue
                }
                arr[i]--
                best = Math.max(best, 1 + solve(i, arr))
                arr[i]++
            }
        } else {
            for (i in 1 until arr.size) {
                if (arr[i] > 0) {
                    arr[i]--
                    best = best.coerceAtLeast(solve((num + i) % arr.size, arr))
                    arr[i]++
                }
            }
        }
        dp[key] = best
        return best
    }

    private fun isFull(arr: IntArray): Boolean {
        var sum = 0
        for (i in 1 until arr.size) {
            sum += arr[i]
        }
        return sum == 0
    }
}
```