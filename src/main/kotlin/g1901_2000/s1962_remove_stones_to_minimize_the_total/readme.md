[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1962\. Remove Stones to Minimize the Total

Medium

You are given a **0-indexed** integer array `piles`, where `piles[i]` represents the number of stones in the <code>i<sup>th</sup></code> pile, and an integer `k`. You should apply the following operation **exactly** `k` times:

*   Choose any `piles[i]` and **remove** `floor(piles[i] / 2)` stones from it.

**Notice** that you can apply the operation on the **same** pile more than once.

Return _the **minimum** possible total number of stones remaining after applying the_ `k` _operations_.

`floor(x)` is the **greatest** integer that is **smaller** than or **equal** to `x` (i.e., rounds `x` down).

**Example 1:**

**Input:** piles = [5,4,9], k = 2

**Output:** 12

**Explanation:** Steps of a possible scenario are: 

- Apply the operation on pile 2. The resulting piles are [5,4,5]. 

- Apply the operation on pile 0. The resulting piles are [3,4,5]. 
  
The total number of stones in [3,4,5] is 12.

**Example 2:**

**Input:** piles = [4,3,6,7], k = 3

**Output:** 12

**Explanation:** Steps of a possible scenario are: 

- Apply the operation on pile 2. The resulting piles are [4,3,3,7]. 

- Apply the operation on pile 3. The resulting piles are [4,3,3,4]. 

- Apply the operation on pile 0. The resulting piles are [2,3,3,4]. 
  
The total number of stones in [2,3,3,4] is 12.

**Constraints:**

*   <code>1 <= piles.length <= 10<sup>5</sup></code>
*   <code>1 <= piles[i] <= 10<sup>4</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.Collections
import java.util.PriorityQueue

@Suppress("NAME_SHADOWING")
class Solution {
    fun minStoneSum(piles: IntArray, k: Int): Int {
        var k = k
        val descendingQueue = PriorityQueue(Collections.reverseOrder<Int>())
        var sum = 0
        var newValue: Int
        var currentValue: Int
        var half: Int
        for (stones in piles) {
            sum += stones
            descendingQueue.offer(stones)
        }
        while (k > 0) {
            currentValue = descendingQueue.poll()
            half = currentValue / 2
            newValue = currentValue - half
            descendingQueue.offer(newValue)
            sum -= half
            k--
        }
        return sum
    }
}
```