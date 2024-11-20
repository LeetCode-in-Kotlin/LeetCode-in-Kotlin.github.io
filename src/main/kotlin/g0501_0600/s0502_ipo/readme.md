[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 502\. IPO

Hard

Suppose LeetCode will start its **IPO** soon. In order to sell a good price of its shares to Venture Capital, LeetCode would like to work on some projects to increase its capital before the **IPO**. Since it has limited resources, it can only finish at most `k` distinct projects before the **IPO**. Help LeetCode design the best way to maximize its total capital after finishing at most `k` distinct projects.

You are given `n` projects where the <code>i<sup>th</sup></code> project has a pure profit `profits[i]` and a minimum capital of `capital[i]` is needed to start it.

Initially, you have `w` capital. When you finish a project, you will obtain its pure profit and the profit will be added to your total capital.

Pick a list of **at most** `k` distinct projects from given projects to **maximize your final capital**, and return _the final maximized capital_.

The answer is guaranteed to fit in a 32-bit signed integer.

**Example 1:**

**Input:** k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]

**Output:** 4

**Explanation:** Since your initial capital is 0, you can only start the project indexed 0. After finishing it you will obtain profit 1 and your capital becomes 1. With capital 1, you can either start the project indexed 1 or the project indexed 2. Since you can choose at most 2 projects, you need to finish the project indexed 2 to get the maximum capital. Therefore, output the final maximized capital, which is 0 + 1 + 3 = 4.

**Example 2:**

**Input:** k = 3, w = 0, profits = [1,2,3], capital = [0,1,2]

**Output:** 6

**Constraints:**

*   <code>1 <= k <= 10<sup>5</sup></code>
*   <code>0 <= w <= 10<sup>9</sup></code>
*   `n == profits.length`
*   `n == capital.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= profits[i] <= 10<sup>4</sup></code>
*   <code>0 <= capital[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    inner class Data(var profit: Int, var capital: Int)

    // max heap for profit
    var profitMaxHeap = PriorityQueue<Data> { d1, d2 ->
        -1 * Integer.compare(
            d1.profit,
            d2.profit,
        )
    }

    // min heap for capital
    var capitalMinHeap = PriorityQueue<Data> { d1, d2 -> Integer.compare(d1.capital, d2.capital) }
    fun findMaximizedCapital(k: Int, w: Int, profits: IntArray, capital: IntArray): Int {
        init(profits, capital)
        var maxCapital = w
        var currentCapital = w
        for (i in 0 until k) {
            // first fetch all tasks you can do with current capital and add those in profit max heap
            while (capitalMinHeap.isNotEmpty() && currentCapital >= capitalMinHeap.peek().capital) {
                profitMaxHeap.add(capitalMinHeap.poll())
            }

            // if profit max heap is empty we can do nothing so break
            if (profitMaxHeap.isEmpty()) break

            // add profit to current capital and update maxCapital
            currentCapital += profitMaxHeap.poll().profit
            maxCapital = Math.max(maxCapital, currentCapital)
        }
        return maxCapital
    }

    private fun init(profits: IntArray, capital: IntArray) {
        for (i in profits.indices) {
            capitalMinHeap.add(Data(profits[i], capital[i]))
        }
    }
}
```