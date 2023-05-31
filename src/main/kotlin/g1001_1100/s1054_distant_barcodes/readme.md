[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1054\. Distant Barcodes

Medium

In a warehouse, there is a row of barcodes, where the <code>i<sup>th</sup></code> barcode is `barcodes[i]`.

Rearrange the barcodes so that no two adjacent barcodes are equal. You may return any answer, and it is guaranteed an answer exists.

**Example 1:**

**Input:** barcodes = [1,1,1,2,2,2]

**Output:** [2,1,2,1,2,1]

**Example 2:**

**Input:** barcodes = [1,1,1,1,2,2,3,3]

**Output:** [1,3,1,3,1,2,1,2]

**Constraints:**

*   `1 <= barcodes.length <= 10000`
*   `1 <= barcodes[i] <= 10000`

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun rearrangeBarcodes(barcodes: IntArray): IntArray {
        val map = barcodes.groupBy { it }.mapValues { it.value.size }
        val pq = PriorityQueue<Pair<Int, Int>> { a, b -> b.second - a.second }
        map.forEach { kv -> pq.offer(kv.toPair()) }
        val result = IntArray(barcodes.size)
        var ind = 0
        while (pq.isNotEmpty()) {
            val remainingBcs = mutableListOf<Pair<Int, Int>>()
            for (i in 0 until 2) {
                if (pq.isNotEmpty()) {
                    val max = pq.poll()
                    result[ind++] = max.first
                    if (max.second - 1 != 0) {
                        remainingBcs.add(Pair(max.first, max.second - 1))
                    }
                }
            }
            remainingBcs.forEach { bc -> pq.offer(bc) }
        }
        return result
    }
}
```