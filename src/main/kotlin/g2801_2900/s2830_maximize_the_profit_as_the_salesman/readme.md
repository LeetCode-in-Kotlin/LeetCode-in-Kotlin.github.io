[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2830\. Maximize the Profit as the Salesman

Medium

You are given an integer `n` representing the number of houses on a number line, numbered from `0` to `n - 1`.

Additionally, you are given a 2D integer array `offers` where <code>offers[i] = [start<sub>i</sub>, end<sub>i</sub>, gold<sub>i</sub>]</code>, indicating that <code>i<sup>th</sup></code> buyer wants to buy all the houses from <code>start<sub>i</sub></code> to <code>end<sub>i</sub></code> for <code>gold<sub>i</sub></code> amount of gold.

As a salesman, your goal is to **maximize** your earnings by strategically selecting and selling houses to buyers.

Return _the maximum amount of gold you can earn_.

**Note** that different buyers can't buy the same house, and some houses may remain unsold.

**Example 1:**

**Input:** n = 5, offers = \[\[0,0,1],[0,2,2],[1,3,2]]

**Output:** 3

**Explanation:** There are 5 houses numbered from 0 to 4 and there are 3 purchase offers. 

We sell houses in the range [0,0] to 1<sup>st</sup> buyer for 1 gold and houses in the range [1,3] to 3<sup>rd</sup> buyer for 2 golds. 

It can be proven that 3 is the maximum amount of gold we can achieve.

**Example 2:**

**Input:** n = 5, offers = \[\[0,0,1],[0,2,10],[1,3,2]]

**Output:** 10

**Explanation:** There are 5 houses numbered from 0 to 4 and there are 3 purchase offers. 

We sell houses in the range [0,2] to 2<sup>nd</sup> buyer for 10 golds. 

It can be proven that 10 is the maximum amount of gold we can achieve.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= offers.length <= 10<sup>5</sup></code>
*   `offers[i].length == 3`
*   <code>0 <= start<sub>i</sub> <= end<sub>i</sub> <= n - 1</code>
*   <code>1 <= gold<sub>i</sub> <= 10<sup>3</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximizeTheProfit(n: Int, offers: List<List<Int>>): Int {
        val dp = IntArray(n)
        val range = HashMap<Int, MutableList<List<Int>>>()
        for (l in offers) {
            if (range.containsKey(l[0])) {
                range[l[0]]!!.add(l)
            } else {
                val r: MutableList<List<Int>> = ArrayList()
                r.add(l)
                range[l[0]] = r
            }
        }
        var i = 0
        while (i < n) {
            var temp: List<List<Int>> = ArrayList()
            if (range.containsKey(i)) {
                temp = range[i]!!
            }
            dp[i] = if ((i != 0)) max(dp[i], dp[i - 1]) else dp[i]
            for (l in temp) {
                dp[l[1]] =
                    if ((i != 0)
                    ) {
                        max(dp[l[1]], (dp[i - 1] + l[2]))
                    } else {
                        max(
                            dp[l[1]],
                            l[2],
                        )
                    }
            }
            i++
        }
        return dp[n - 1]
    }
}
```