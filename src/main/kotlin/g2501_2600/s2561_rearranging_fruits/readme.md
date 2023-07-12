[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2561\. Rearranging Fruits

Hard

You have two fruit baskets containing `n` fruits each. You are given two **0-indexed** integer arrays `basket1` and `basket2` representing the cost of fruit in each basket. You want to make both baskets **equal**. To do so, you can use the following operation as many times as you want:

*   Chose two indices `i` and `j`, and swap the <code>i<sup>th</sup></code> fruit of `basket1` with the <code>j<sup>th</sup></code> fruit of `basket2`.
*   The cost of the swap is `min(basket1[i],basket2[j])`.

Two baskets are considered equal if sorting them according to the fruit cost makes them exactly the same baskets.

Return _the minimum cost to make both the baskets equal or_ `-1` _if impossible._

**Example 1:**

**Input:** basket1 = [4,2,2,2], basket2 = [1,4,1,2]

**Output:** 1

**Explanation:** Swap index 1 of basket1 with index 0 of basket2, which has cost 1. Now basket1 = [4,1,2,2] and basket2 = [2,4,1,2]. Rearranging both the arrays makes them equal.

**Example 2:**

**Input:** basket1 = [2,3,4,1], basket2 = [3,2,5,1]

**Output:** -1

**Explanation:** It can be shown that it is impossible to make both the baskets equal.

**Constraints:**

*   `basket1.length == bakste2.length`
*   <code>1 <= basket1.length <= 10<sup>5</sup></code>
*   <code>1 <= basket1[i],basket2[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minCost(basket1: IntArray, basket2: IntArray): Long {
        val n = basket1.size
        val map1: MutableMap<Int, Int> = HashMap()
        val map2: MutableMap<Int, Int> = HashMap()
        var minVal = Int.MAX_VALUE // Use on indirect swap

        // Counting the basket's number to each HashMap
        for (i in 0 until n) {
            map1[basket1[i]] = map1.getOrDefault(basket1[i], 0) + 1
            map2[basket2[i]] = map2.getOrDefault(basket2[i], 0) + 1
            minVal = minVal.coerceAtMost(basket1[i])
            minVal = minVal.coerceAtMost(basket2[i])
        }

        // build swap list, if any number is too more, add numbers to prepare swap list
        val swapList1: MutableList<Int> = ArrayList()
        for (key in map1.keys) {
            val c1 = map1[key]!!
            val c2 = map2.getOrDefault(key, 0)
            if ((c1 + c2) % 2 == 1) return -1
            if (c1 > c2) {
                var addCnt = (c1 - c2) / 2
                while (addCnt-- > 0) {
                    swapList1.add(key)
                }
            }
        }
        val swapList2: MutableList<Int> = ArrayList()
        for (key in map2.keys) {
            val c1 = map1.getOrDefault(key, 0)
            val c2 = map2[key]!!
            if ((c1 + c2) % 2 == 1) return -1
            if (c2 > c1) {
                var addCnt = (c2 - c1) / 2
                while (addCnt-- > 0) {
                    swapList2.add(key)
                }
            }
        }

        // Sorting
        swapList1.sort()
        swapList2.sortWith { a: Int, b: Int -> b - a }

        // visite swap list
        var res: Long = 0
        for (i in swapList1.indices) {
            // Two choices to swap, direct swap or indirect swap
            res += (2 * minVal).coerceAtMost(swapList1[i].coerceAtMost(swapList2[i])).toLong()
        }
        return res
    }
}
```