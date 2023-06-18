[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 638\. Shopping Offers

Medium

In LeetCode Store, there are `n` items to sell. Each item has a price. However, there are some special offers, and a special offer consists of one or more different kinds of items with a sale price.

You are given an integer array `price` where `price[i]` is the price of the <code>i<sup>th</sup></code> item, and an integer array `needs` where `needs[i]` is the number of pieces of the <code>i<sup>th</sup></code> item you want to buy.

You are also given an array `special` where `special[i]` is of size `n + 1` where `special[i][j]` is the number of pieces of the <code>j<sup>th</sup></code> item in the <code>i<sup>th</sup></code> offer and `special[i][n]` (i.e., the last integer in the array) is the price of the <code>i<sup>th</sup></code> offer.

Return _the lowest price you have to pay for exactly certain items as given, where you could make optimal use of the special offers_. You are not allowed to buy more items than you want, even if that would lower the overall price. You could use any of the special offers as many times as you want.

**Example 1:**

**Input:** price = [2,5], special = \[\[3,0,5],[1,2,10]], needs = [3,2]

**Output:** 14

**Explanation:** There are two kinds of items, A and B. Their prices are $2 and $5 respectively. 

In special offer 1, you can pay $5 for 3A and 0B In special offer 2, you can pay $10 for 1A and 2B. 

You need to buy 3A and 2B, so you may pay $10 for 1A and 2B (special offer #2), and $4 for 2A.

**Example 2:**

**Input:** price = [2,3,4], special = \[\[1,1,0,4],[2,2,1,9]], needs = [1,2,1]

**Output:** 11

**Explanation:** The price of A is $2, and $3 for B, $4 for C.

You may pay $4 for 1A and 1B, and $9 for 2A ,2B and 1C. 

You need to buy 1A ,2B and 1C, so you may pay $4 for 1A and 1B (special offer #1), and $3 for 1B, $4 for 1C. 

You cannot add more items, though only $9 for 2A ,2B and 1C.

**Constraints:**

*   `n == price.length == needs.length`
*   `1 <= n <= 6`
*   `0 <= price[i], needs[i] <= 10`
*   `1 <= special.length <= 100`
*   `special[i].length == n + 1`
*   `0 <= special[i][j] <= 50`

## Solution

```kotlin
class Solution {
    fun shoppingOffers(
        price: List<Int>,
        special: List<List<Int>>,
        needs: List<Int>
    ): Int {
        val map: MutableMap<List<Int>, Int> = HashMap()
        shoppingOffersUtil(price, special, needs, map)
        return map[needs]!!
    }

    private fun shoppingOffersUtil(
        price: List<Int>,
        special: List<List<Int>>,
        needs: List<Int>,
        map: MutableMap<List<Int>, Int>
    ): Int {
        if (map.containsKey(needs)) {
            return map[needs]!!
        }
        var ans = computePrice(price, needs)
        for (i in special.indices) {
            if (verify(special[i], needs)) {
                ans = Math.min(
                    special[i][needs.size] +
                        shoppingOffersUtil(
                            price,
                            special,
                            updatedNeeds(needs, special[i]),
                            map
                        ),
                    ans
                )
            }
        }
        map[needs] = ans
        return (map[needs])!!
    }

    private fun updatedNeeds(needs: List<Int>, special: List<Int>): List<Int> {
        val updatedNeeds: MutableList<Int> = ArrayList(needs)
        for (i in needs.indices) {
            updatedNeeds[i] = updatedNeeds[i] - special[i]
        }
        return updatedNeeds
    }

    private fun verify(special: List<Int>, needs: List<Int>): Boolean {
        for (i in needs.indices) {
            if (special[i] > needs[i]) {
                return false
            }
        }
        return true
    }

    private fun computePrice(price: List<Int>, needs: List<Int>): Int {
        var ans = 0
        for (i in needs.indices) {
            ans += (needs[i] * price[i])
        }
        return ans
    }
}
```