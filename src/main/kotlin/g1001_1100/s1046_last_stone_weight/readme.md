[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1046\. Last Stone Weight

Easy

You are given an array of integers `stones` where `stones[i]` is the weight of the <code>i<sup>th</sup></code> stone.

We are playing a game with the stones. On each turn, we choose the **heaviest two stones** and smash them together. Suppose the heaviest two stones have weights `x` and `y` with `x <= y`. The result of this smash is:

*   If `x == y`, both stones are destroyed, and
*   If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is **at most one** stone left.

Return _the weight of the last remaining stone_. If there are no stones left, return `0`.

**Example 1:**

**Input:** stones = [2,7,4,1,8,1]

**Output:** 1

**Explanation:** 

We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then, 

we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,

we combine 2 and 1 to get 1 so the array converts to [1,1,1] then, 

we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of the last stone.

**Example 2:**

**Input:** stones = [1]

**Output:** 1

**Constraints:**

*   `1 <= stones.length <= 30`
*   `1 <= stones[i] <= 1000`

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun lastStoneWeight(stones: IntArray): Int {
        val heap = PriorityQueue { a: Int, b: Int -> b - a }
        for (stone in stones) {
            heap.offer(stone)
        }
        while (heap.isNotEmpty()) {
            if (heap.size >= 2) {
                val one = heap.poll()
                val two = heap.poll()
                val diff = one - two
                heap.offer(diff)
            } else {
                return heap.poll()
            }
        }
        return -1
    }
}
```