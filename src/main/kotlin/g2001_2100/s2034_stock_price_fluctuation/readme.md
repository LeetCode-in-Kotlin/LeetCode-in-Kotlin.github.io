[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2034\. Stock Price Fluctuation

Medium

You are given a stream of **records** about a particular stock. Each record contains a **timestamp** and the corresponding **price** of the stock at that timestamp.

Unfortunately due to the volatile nature of the stock market, the records do not come in order. Even worse, some records may be incorrect. Another record with the same timestamp may appear later in the stream **correcting** the price of the previous wrong record.

Design an algorithm that:

*   **Updates** the price of the stock at a particular timestamp, **correcting** the price from any previous records at the timestamp.
*   Finds the **latest price** of the stock based on the current records. The **latest price** is the price at the latest timestamp recorded.
*   Finds the **maximum price** the stock has been based on the current records.
*   Finds the **minimum price** the stock has been based on the current records.

Implement the `StockPrice` class:

*   `StockPrice()` Initializes the object with no price records.
*   `void update(int timestamp, int price)` Updates the `price` of the stock at the given `timestamp`.
*   `int current()` Returns the **latest price** of the stock.
*   `int maximum()` Returns the **maximum price** of the stock.
*   `int minimum()` Returns the **minimum price** of the stock.

**Example 1:**

**Input** ["StockPrice", "update", "update", "current", "maximum", "update", "maximum", "update", "minimum"] [[], [1, 10], [2, 5], [], [], [1, 3], [], [4, 2], []]

**Output:** [null, null, null, 5, 10, null, 5, null, 2]

**Explanation:** 

StockPrice stockPrice = new StockPrice(); 

stockPrice.update(1, 10); // Timestamps are [1] with corresponding prices [10]. 

stockPrice.update(2, 5); // Timestamps are [1,2] with corresponding prices [10,5]. 

stockPrice.current(); // return 5, the latest timestamp is 2 with the price being 5. 

stockPrice.maximum(); // return 10, the maximum price is 10 at timestamp 1. 

stockPrice.update(1, 3); // The previous timestamp 1 had the wrong price, so it is updated to 3. // Timestamps are [1,2] with corresponding prices [3,5]. 

stockPrice.maximum(); // return 5, the maximum price is 5 after the correction. 

stockPrice.update(4, 2); // Timestamps are [1,2,4] with corresponding prices [3,5,2]. 

stockPrice.minimum(); // return 2, the minimum price is 2 at timestamp 4.

**Constraints:**

*   <code>1 <= timestamp, price <= 10<sup>9</sup></code>
*   At most <code>10<sup>5</sup></code> calls will be made **in total** to `update`, `current`, `maximum`, and `minimum`.
*   `current`, `maximum`, and `minimum` will be called **only after** `update` has been called **at least once**.

## Solution

```kotlin
import java.util.PriorityQueue

class StockPrice {
    private class Record(var time: Int, var price: Int)

    private val map: MutableMap<Int, Int>
    private val maxHeap: PriorityQueue<Record>
    private val minHeap: PriorityQueue<Record>
    private var latestTimestamp = 0

    init {
        map = HashMap()
        maxHeap = PriorityQueue { r1: Record, r2: Record -> Integer.compare(r2.price, r1.price) }
        minHeap = PriorityQueue { r1: Record, r2: Record -> Integer.compare(r1.price, r2.price) }
    }

    fun update(timestamp: Int, price: Int) {
        latestTimestamp = Math.max(timestamp, latestTimestamp)
        maxHeap.offer(Record(timestamp, price))
        minHeap.offer(Record(timestamp, price))
        map[timestamp] = price
    }

    fun current(): Int {
        return map[latestTimestamp]!!
    }

    fun maximum(): Int {
        while (true) {
            val rec = maxHeap.peek()
            if (map[rec.time] == rec.price) {
                return rec.price
            }
            maxHeap.poll()
        }
    }

    fun minimum(): Int {
        while (true) {
            val rec = minHeap.peek()
            if (map[rec.time] == rec.price) {
                return rec.price
            }
            minHeap.poll()
        }
    }
}
/*
 * Your StockPrice object will be instantiated and called as such:
 * var obj = StockPrice()
 * obj.update(timestamp,price)
 * var param_2 = obj.current()
 * var param_3 = obj.maximum()
 * var param_4 = obj.minimum()
 */
```