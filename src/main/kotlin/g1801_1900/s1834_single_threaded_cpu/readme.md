[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1834\. Single-Threaded CPU

Medium

You are given `n` tasks labeled from `0` to `n - 1` represented by a 2D integer array `tasks`, where <code>tasks[i] = [enqueueTime<sub>i</sub>, processingTime<sub>i</sub>]</code> means that the <code>i<sup>th</sup></code> task will be available to process at <code>enqueueTime<sub>i</sub></code> and will take <code>processingTime<sub>i</sub></code> to finish processing.

You have a single-threaded CPU that can process **at most one** task at a time and will act in the following way:

*   If the CPU is idle and there are no available tasks to process, the CPU remains idle.
*   If the CPU is idle and there are available tasks, the CPU will choose the one with the **shortest processing time**. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
*   Once a task is started, the CPU will **process the entire task** without stopping.
*   The CPU can finish a task then start a new one instantly.

Return _the order in which the CPU will process the tasks._

**Example 1:**

**Input:** tasks = \[\[1,2],[2,4],[3,2],[4,1]]

**Output:** [0,2,3,1]

**Explanation:** The events go as follows: 

- At time = 1, task 0 is available to process. Available tasks = {0}. 

- Also at time = 1, the idle CPU starts processing task 0. Available tasks = {}. 

- At time = 2, task 1 is available to process. Available tasks = {1}.

- At time = 3, task 2 is available to process. Available tasks = {1, 2}. 

- Also at time = 3, the CPU finishes task 0 and starts processing task 2 as it is the shortest. Available tasks = {1}. 

- At time = 4, task 3 is available to process. Available tasks = {1, 3}. - At time = 5, the CPU finishes task 2 and starts processing task 3 as it is the shortest. Available tasks = {1}. 

- At time = 6, the CPU finishes task 3 and starts processing task 1. Available tasks = {}. 

- At time = 10, the CPU finishes task 1 and becomes idle.

**Example 2:**

**Input:** tasks = \[\[7,10],[7,12],[7,5],[7,4],[7,2]]

**Output:** [4,3,2,0,1]

**Explanation:** The events go as follows: 

- At time = 7, all the tasks become available. Available tasks = {0,1,2,3,4}. 

- Also at time = 7, the idle CPU starts processing task 4. Available tasks = {0,1,2,3}. 

- At time = 9, the CPU finishes task 4 and starts processing task 3. Available tasks = {0,1,2}. 

- At time = 13, the CPU finishes task 3 and starts processing task 2. Available tasks = {0,1}. 

- At time = 18, the CPU finishes task 2 and starts processing task 0. Available tasks = {1}. 

- At time = 28, the CPU finishes task 0 and starts processing task 1. Available tasks = {}. 

- At time = 40, the CPU finishes task 1 and becomes idle.

**Constraints:**

*   `tasks.length == n`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= enqueueTime<sub>i</sub>, processingTime<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun getOrder(tasks1: Array<IntArray>): IntArray {
        val n = tasks1.size
        val tasks = Array(n) { IntArray(3) }
        for (i in 0 until n) {
            tasks[i] = intArrayOf(tasks1[i][0], tasks1[i][1], i)
        }
        tasks.sortWith(compareBy { a: IntArray -> a[0] })
        val minHeap = PriorityQueue(
            Comparator { a: IntArray, b: IntArray ->
                return@Comparator if (a[1] == b[1]) {
                    a[2] - b[2]
                } else {
                    a[1] - b[1]
                }
            },
        )
        var time = tasks[0][0]
        val taskOrderResult = IntArray(n)
        var i = 0
        var index = 0
        while (minHeap.isNotEmpty() || i < n) {
            while (i < n && time >= tasks[i][0]) {
                minHeap.add(tasks[i++])
            }
            if (minHeap.isNotEmpty()) {
                val task = minHeap.remove()
                taskOrderResult[index++] = task[2]
                time += task[1]
            } else {
                time = tasks[i][0]
            }
        }
        return taskOrderResult
    }
}
```