[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 397\. Integer Replacement

Medium

Given a positive integer `n`, you can apply one of the following operations:

1.  If `n` is even, replace `n` with `n / 2`.
2.  If `n` is odd, replace `n` with either `n + 1` or `n - 1`.

Return _the minimum number of operations needed for_ `n` _to become_ `1`.

**Example 1:**

**Input:** n = 8

**Output:** 3

**Explanation:** 8 -> 4 -> 2 -> 1

**Example 2:**

**Input:** n = 7

**Output:** 4

**Explanation:** 7 -> 8 -> 4 -> 2 -> 1 or 7 -> 6 -> 3 -> 2 -> 1

**Example 3:**

**Input:** n = 4

**Output:** 2

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
import java.util.LinkedList

class Solution {
    fun integerReplacement(n: Int): Int {
        if (n == 1) return 0
        val num = n.toLong()
        val queue = LinkedList<Long>()
        val seen = HashSet<Long>()
        if (n % 2 == 0) {
            queue.offer(num / 2)
            seen.add(num / 2)
        } else {
            queue.offer(num + 1)
            seen.add(num + 1)
            queue.offer(num - 1)
            seen.add(num - 1)
        }
        var steps = 1
        while (queue.isNotEmpty()) {
            val size = queue.size
            for (sz in 0 until size) {
                val cur = queue.poll()
                if (cur == 1L) return steps
                if (cur % 2 == 0L) {
                    val next = cur / 2
                    if (seen.add(next)) {
                        queue.offer(next)
                    }
                } else {
                    val next1 = cur + 1
                    if (seen.add(next1)) {
                        queue.offer(next1)
                    }
                    val next2 = cur - 1
                    if (seen.add(next2)) {
                        queue.offer(next2)
                    }
                }
            }
            ++steps
        }
        return steps
    }
}
```