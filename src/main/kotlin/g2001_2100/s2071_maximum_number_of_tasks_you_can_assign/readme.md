[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2071\. Maximum Number of Tasks You Can Assign

Hard

You have `n` tasks and `m` workers. Each task has a strength requirement stored in a **0-indexed** integer array `tasks`, with the <code>i<sup>th</sup></code> task requiring `tasks[i]` strength to complete. The strength of each worker is stored in a **0-indexed** integer array `workers`, with the <code>j<sup>th</sup></code> worker having `workers[j]` strength. Each worker can only be assigned to a **single** task and must have a strength **greater than or equal** to the task's strength requirement (i.e., `workers[j] >= tasks[i]`).

Additionally, you have `pills` magical pills that will **increase a worker's strength** by `strength`. You can decide which workers receive the magical pills, however, you may only give each worker **at most one** magical pill.

Given the **0-indexed** integer arrays `tasks` and `workers` and the integers `pills` and `strength`, return _the **maximum** number of tasks that can be completed._

**Example 1:**

**Input:** tasks = [**3**,**2**,**1**], workers = [**0**,**3**,**3**], pills = 1, strength = 1

**Output:** 3

**Explanation:**

We can assign the magical pill and tasks as follows:

- Give the magical pill to worker 0.

- Assign worker 0 to task 2 (0 + 1 >= 1)

- Assign worker 1 to task 1 (3 >= 2)

- Assign worker 2 to task 0 (3 >= 3) 

**Example 2:**

**Input:** tasks = [**5**,4], workers = [**0**,0,0], pills = 1, strength = 5

**Output:** 1

**Explanation:**

We can assign the magical pill and tasks as follows:

- Give the magical pill to worker 0.

- Assign worker 0 to task 0 (0 + 5 >= 5) 

**Example 3:**

**Input:** tasks = [**10**,**15**,30], workers = [**0**,**10**,10,10,10], pills = 3, strength = 10

**Output:** 2

**Explanation:**

We can assign the magical pills and tasks as follows:

- Give the magical pill to worker 0 and worker 1.

- Assign worker 0 to task 0 (0 + 10 >= 10)

- Assign worker 1 to task 1 (10 + 10 >= 15)

The last pill is not given because it will not make any worker strong enough for the last task. 

**Constraints:**

*   `n == tasks.length`
*   `m == workers.length`
*   <code>1 <= n, m <= 5 * 10<sup>4</sup></code>
*   `0 <= pills <= m`
*   <code>0 <= tasks[i], workers[j], strength <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

@Suppress("NAME_SHADOWING")
class Solution {
    fun maxTaskAssign(tasks: IntArray, workers: IntArray, pills: Int, strength: Int): Int {
        var left = 0
        var right = tasks.size.coerceAtMost(workers.size)
        tasks.sort()
        workers.sort()
        while (left + 1 < right) {
            val mid = left + (right - left) / 2
            if (canAssign(mid, tasks, workers, pills, strength)) {
                left = mid
            } else {
                right = mid
            }
        }
        return if (canAssign(right, tasks, workers, pills, strength)) {
            right
        } else {
            left
        }
    }

    private fun canAssign(count: Int, tasks: IntArray, workers: IntArray, pills: Int, strength: Int): Boolean {
        var pills = pills
        val dq: Deque<Int> = LinkedList()
        var ind = workers.size - 1
        for (i in count - 1 downTo 0) {
            while (ind >= workers.size - count && workers[ind] + strength >= tasks[i]) {
                dq.offerLast(workers[ind])
                ind--
            }
            if (dq.isEmpty()) {
                return false
            }
            if (dq.peekFirst() >= tasks[i]) {
                dq.pollFirst()
            } else {
                dq.pollLast()
                pills--
                if (pills < 0) {
                    return false
                }
            }
        }
        return true
    }
}
```