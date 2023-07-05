[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2532\. Time to Cross a Bridge

Hard

There are `k` workers who want to move `n` boxes from an old warehouse to a new one. You are given the two integers `n` and `k`, and a 2D integer array `time` of size `k x 4` where <code>time[i] = [leftToRight<sub>i</sub>, pickOld<sub>i</sub>, rightToLeft<sub>i</sub>, putNew<sub>i</sub>]</code>.

The warehouses are separated by a river and connected by a bridge. The old warehouse is on the right bank of the river, and the new warehouse is on the left bank of the river. Initially, all `k` workers are waiting on the left side of the bridge. To move the boxes, the <code>i<sup>th</sup></code> worker (**0-indexed**) can :

*   Cross the bridge from the left bank (new warehouse) to the right bank (old warehouse) in <code>leftToRight<sub>i</sub></code> minutes.
*   Pick a box from the old warehouse and return to the bridge in <code>pickOld<sub>i</sub></code> minutes. Different workers can pick up their boxes simultaneously.
*   Cross the bridge from the right bank (old warehouse) to the left bank (new warehouse) in <code>rightToLeft<sub>i</sub></code> minutes.
*   Put the box in the new warehouse and return to the bridge in <code>putNew<sub>i</sub></code> minutes. Different workers can put their boxes simultaneously.

A worker `i` is **less efficient** than a worker `j` if either condition is met:

*   <code>leftToRight<sub>i</sub> + rightToLeft<sub>i</sub> > leftToRight<sub>j</sub> + rightToLeft<sub>j</sub></code>
*   <code>leftToRight<sub>i</sub> + rightToLeft<sub>i</sub> == leftToRight<sub>j</sub> + rightToLeft<sub>j</sub></code> and `i > j`

The following rules regulate the movement of the workers through the bridge :

*   If a worker `x` reaches the bridge while another worker `y` is crossing the bridge, `x` waits at their side of the bridge.
*   If the bridge is free, the worker waiting on the right side of the bridge gets to cross the bridge. If more than one worker is waiting on the right side, the one with **the lowest efficiency** crosses first.
*   If the bridge is free and no worker is waiting on the right side, and at least one box remains at the old warehouse, the worker on the left side of the river gets to cross the bridge. If more than one worker is waiting on the left side, the one with **the lowest efficiency** crosses first.

Return _the instance of time at which the last worker **reaches the left bank** of the river after all n boxes have been put in the new warehouse_.

**Example 1:**

**Input:** n = 1, k = 3, time = \[\[1,1,2,1],[1,1,3,1],[1,1,4,1]]

**Output:** 6

**Explanation:**  

From 0 to 1: worker 2 crosses the bridge from the left bank to the right bank.

From 1 to 2: worker 2 picks up a box from the old warehouse.

From 2 to 6: worker 2 crosses the bridge from the right bank to the left bank. 

From 6 to 7: worker 2 puts a box at the new warehouse. 

The whole process ends after 7 minutes. We return 6 because the problem asks for the instance of time at which the last worker reaches the left bank.

**Example 2:**

**Input:** n = 3, k = 2, time = \[\[1,9,1,8],[10,10,10,10]]

**Output:** 50

**Explanation:** 

From 0 to 10: worker 1 crosses the bridge from the left bank to the right bank. 

From 10 to 20: worker 1 picks up a box from the old warehouse. 

From 10 to 11: worker 0 crosses the bridge from the left bank to the right bank.

From 11 to 20: worker 0 picks up a box from the old warehouse. 

From 20 to 30: worker 1 crosses the bridge from the right bank to the left bank.

From 30 to 40: worker 1 puts a box at the new warehouse. 

From 30 to 31: worker 0 crosses the bridge from the right bank to the left bank. 

From 31 to 39: worker 0 puts a box at the new warehouse.

From 39 to 40: worker 0 crosses the bridge from the left bank to the right bank. 

From 40 to 49: worker 0 picks up a box from the old warehouse. 

From 49 to 50: worker 0 crosses the bridge from the right bank to the left bank.

From 50 to 58: worker 0 puts a box at the new warehouse. 

The whole process ends after 58 minutes. We return 50 because the problem asks for the instance of time at which the last worker reaches the left bank.

**Constraints:**

*   <code>1 <= n, k <= 10<sup>4</sup></code>
*   `time.length == k`
*   `time[i].length == 4`
*   <code>1 <= leftToRight<sub>i</sub>, pickOld<sub>i</sub>, rightToLeft<sub>i</sub>, putNew<sub>i</sub> <= 1000</code>

## Solution

```kotlin
import java.util.PriorityQueue

@Suppress("NAME_SHADOWING")
class Solution {
    fun findCrossingTime(n: Int, k: Int, time: Array<IntArray>): Int {
        // create 2 PQ
        var n = n
        val leftBridgePQ = PriorityQueue { a: IntArray, b: IntArray -> if (a[1] == b[1]) b[0] - a[0] else b[1] - a[1] }
        val rightBridgePQ = PriorityQueue { a: IntArray, b: IntArray -> if (a[1] == b[1]) b[0] - a[0] else b[1] - a[1] }
        val leftWHPQ = PriorityQueue { a: IntArray, b: IntArray -> a[1].compareTo(b[1]) }
        val rightWHPQ = PriorityQueue { a: IntArray, b: IntArray -> a[1].compareTo(b[1]) }
        for (i in 0 until k) {
            val efficiency = time[i][0] + time[i][2]
            leftBridgePQ.offer(intArrayOf(i, efficiency))
        }
        var duration = 0
        while (n > 0 || rightBridgePQ.isNotEmpty() || rightWHPQ.isNotEmpty()) {
            while (leftWHPQ.isNotEmpty() && leftWHPQ.peek()[1] <= duration) {
                val id = leftWHPQ.poll()[0]
                val e = time[id][0] + time[id][2]
                leftBridgePQ.offer(intArrayOf(id, e))
            }
            while (rightWHPQ.isNotEmpty() && rightWHPQ.peek()[1] <= duration) {
                val id = rightWHPQ.poll()[0]
                val e = time[id][0] + time[id][2]
                rightBridgePQ.offer(intArrayOf(id, e))
            }
            if (rightBridgePQ.isNotEmpty()) {
                val id = rightBridgePQ.poll()[0]
                duration += time[id][2]
                leftWHPQ.offer(intArrayOf(id, duration + time[id][3]))
            } else if (leftBridgePQ.isNotEmpty() && n > 0) {
                val id = leftBridgePQ.poll()[0]
                duration += time[id][0]
                rightWHPQ.offer(intArrayOf(id, duration + time[id][1]))
                --n
            } else {
                // update duration
                var left = Int.MAX_VALUE
                if (leftWHPQ.isNotEmpty() && n > 0) {
                    left = leftWHPQ.peek()[1]
                }
                var right = Int.MAX_VALUE
                if (rightWHPQ.isNotEmpty()) {
                    right = rightWHPQ.peek()[1]
                }
                duration = Math.min(left, right)
            }
        }
        return duration
    }
}
```