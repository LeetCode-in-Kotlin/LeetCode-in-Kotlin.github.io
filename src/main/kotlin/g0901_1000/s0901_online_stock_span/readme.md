[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 901\. Online Stock Span

Medium

Design an algorithm that collects daily price quotes for some stock and returns **the span** of that stock's price for the current day.

The **span** of the stock's price today is defined as the maximum number of consecutive days (starting from today and going backward) for which the stock price was less than or equal to today's price.

*   For example, if the price of a stock over the next `7` days were `[100,80,60,70,60,75,85]`, then the stock spans would be `[1,1,1,2,1,4,6]`.

Implement the `StockSpanner` class:

*   `StockSpanner()` Initializes the object of the class.
*   `int next(int price)` Returns the **span** of the stock's price given that today's price is `price`.

**Example 1:**

**Input**

["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]

[[], [100], [80], [60], [70], [60], [75], [85]]

**Output:** [null, 1, 1, 1, 2, 1, 4, 6]

**Explanation:**

    StockSpanner stockSpanner = new StockSpanner(); 
    stockSpanner.next(100); // return 1 
    stockSpanner.next(80); // return 1 
    stockSpanner.next(60); // return 1 
    stockSpanner.next(70); // return 2 
    stockSpanner.next(60); // return 1 
    stockSpanner.next(75); // return 4, because the last 4 prices (including today's price of 75) were less than or equal to today's price. 
    stockSpanner.next(85); // return 6

**Constraints:**

*   <code>1 <= price <= 10<sup>5</sup></code>
*   At most <code>10<sup>4</sup></code> calls will be made to `next`.

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class StockSpanner {
    private val map: MutableMap<Int, Int>
    private val stocks: Deque<Int>
    private var index: Int

    init {
        stocks = LinkedList()
        index = -1
        map = HashMap()
        stocks.push(-1)
    }

    fun next(price: Int): Int {
        if (index != -1) {
            stocks.push(index)
        }
        map[++index] = price
        if (stocks.size == 1) {
            return index - stocks.peek()
        }
        while (stocks.size > 1 && map.getValue(stocks.peek()) <= price) {
            stocks.pop()
        }
        return index - stocks.peek()
    }
}
/*
 * Your StockSpanner object will be instantiated and called as such:
 * var obj = StockSpanner()
 * var param_1 = obj.next(price)
 */
```