[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3377\. Digit Operations to Make Two Integers Equal

Medium

You are given two integers `n` and `m` that consist of the **same** number of digits.

You can perform the following operations **any** number of times:

*   Choose **any** digit from `n` that is not 9 and **increase** it by 1.
*   Choose **any** digit from `n` that is not 0 and **decrease** it by 1.

The integer `n` must not be a **prime** number at any point, including its original value and after each operation.

The cost of a transformation is the sum of **all** values that `n` takes throughout the operations performed.

Return the **minimum** cost to transform `n` into `m`. If it is impossible, return -1.

A prime number is a natural number greater than 1 with only two factors, 1 and itself.

**Example 1:**

**Input:** n = 10, m = 12

**Output:** 85

**Explanation:**

We perform the following operations:

*   Increase the first digit, now <code>n = <ins>**2**</ins>0</code>.
*   Increase the second digit, now <code>n = 2**<ins>1</ins>**</code>.
*   Increase the second digit, now <code>n = 2**<ins>2</ins>**</code>.
*   Decrease the first digit, now <code>n = **<ins>1</ins>**2</code>.

**Example 2:**

**Input:** n = 4, m = 8

**Output:** \-1

**Explanation:**

It is impossible to make `n` equal to `m`.

**Example 3:**

**Input:** n = 6, m = 2

**Output:** \-1

**Explanation:**

Since 2 is already a prime, we can't make `n` equal to `m`.

**Constraints:**

*   <code>1 <= n, m < 10<sup>4</sup></code>
*   `n` and `m` consist of the same number of digits.

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun minOperations(n: Int, m: Int): Int {
        val limit = 100000
        val sieve = BooleanArray(limit + 1)
        val visited = BooleanArray(limit)
        sieve.fill(true)
        sieve[0] = false
        sieve[1] = false
        var i = 2
        while (i * i <= limit) {
            if (sieve[i]) {
                var j = i * i
                while (j <= limit) {
                    sieve[j] = false
                    j += i
                }
            }
            i++
        }
        if (sieve[n]) {
            return -1
        }
        val pq = PriorityQueue<IntArray>(Comparator { a: IntArray, b: IntArray -> a[0] - b[0] })
        visited[n] = true
        pq.add(intArrayOf(n, n))
        while (pq.isNotEmpty()) {
            val current = pq.poll()
            val cost = current[0]
            val num = current[1]
            val temp = num.toString().toCharArray()
            if (num == m) {
                return cost
            }
            for (j in temp.indices) {
                val old = temp[j]
                for (i in -1..1) {
                    val digit = old.code - '0'.code
                    if ((digit == 9 && i == 1) || (digit == 0 && i == -1)) {
                        continue
                    }
                    temp[j] = (i + digit + '0'.code).toChar()
                    val newNum = String(temp).toInt()
                    if (!sieve[newNum] && !visited[newNum]) {
                        visited[newNum] = true
                        pq.add(intArrayOf(cost + newNum, newNum))
                    }
                }
                temp[j] = old
            }
        }
        return -1
    }
}
```