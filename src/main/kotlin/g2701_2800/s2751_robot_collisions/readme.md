[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2751\. Robot Collisions

Hard

There are `n` **1-indexed** robots, each having a position on a line, health, and movement direction.

You are given **0-indexed** integer arrays `positions`, `healths`, and a string `directions` (`directions[i]` is either **'L'** for **left** or **'R'** for **right**). All integers in `positions` are **unique**.

All robots start moving on the line **simultaneously** at the **same speed** in their given directions. If two robots ever share the same position while moving, they will **collide**.

If two robots collide, the robot with **lower health** is **removed** from the line, and the health of the other robot **decreases** **by one**. The surviving robot continues in the **same** direction it was going. If both robots have the **same** health, they are both removed from the line.

Your task is to determine the **health** of the robots that survive the collisions, in the same **order** that the robots were given, i.e. final heath of robot 1 (if survived), final health of robot 2 (if survived), and so on. If there are no survivors, return an empty array.

Return _an array containing the health of the remaining robots (in the order they were given in the input), after no further collisions can occur._

**Note:** The positions may be unsorted.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/05/15/image-20230516011718-12.png)

**Input:** positions = [5,4,3,2,1], healths = [2,17,9,15,10], directions = "RRRRR"

**Output:** [2,17,9,15,10]

**Explanation:** No collision occurs in this example, since all robots are moving in the same direction. So, the health of the robots in order from the first robot is returned, [2, 17, 9, 15, 10].

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/05/15/image-20230516004433-7.png)

**Input:** positions = [3,5,2,6], healths = [10,10,15,12], directions = "RLRL"

**Output:** [14]

**Explanation:** There are 2 collisions in this example. Firstly, robot 1 and robot 2 will collide, and since both have the same health, they will be removed from the line. Next, robot 3 and robot 4 will collide and since robot 4's health is smaller, it gets removed, and robot 3's health becomes 15 - 1 = 14. Only robot 3 remains, so we return [14].

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/05/15/image-20230516005114-9.png)

**Input:** positions = [1,2,5,6], healths = [10,10,11,11], directions = "RLRL"

**Output:** []

**Explanation:** Robot 1 and robot 2 will collide and since both have the same health, they are both removed. Robot 3 and 4 will collide and since both have the same health, they are both removed. So, we return an empty array, [].

**Constraints:**

*   <code>1 <= positions.length == healths.length == directions.length == n <= 10<sup>5</sup></code>
*   <code>1 <= positions[i], healths[i] <= 10<sup>9</sup></code>
*   `directions[i] == 'L'` or `directions[i] == 'R'`
*   All values in `positions` are distinct

## Solution

```kotlin
import java.util.ArrayDeque

class Solution {
    fun survivedRobotsHealths(pos: IntArray, h: IntArray, dir: String): List<Int> {
        val a = Array(pos.size) { IntArray(4) { 0 } }
        for (i in pos.indices) {
            a[i][0] = pos[i]
            a[i][1] = h[i]
            a[i][2] = if (dir[i] == 'R') 1 else 0
            a[i][3] = i
        }
        a.sortWith(compareBy { it[0] })
        val q = ArrayDeque<Int>()
        for (i in a.indices) {
            if (q.isEmpty() || a[i][2] == 1) {
                q.push(i)
            } else {
                var prev = a[q.peek()]
                if (prev[2] == 1) {
                    if (a[i][1] == prev[1]) {
                        q.pop()
                        continue
                    } else {
                        while (true) {
                            if (a[i][1] == prev[1]) {
                                q.pop()
                                break
                            }
                            if (prev[1] > a[i][1]) {
                                prev[1] -= 1
                                break
                            } else {
                                q.pop()
                                a[i][1] -= 1
                                if (q.isEmpty() || a[q.peek()][2] == 0) {
                                    q.push(i)
                                    break
                                } else {
                                    prev = a[q.peek()]
                                }
                            }
                        }
                    }
                } else {
                    q.push(i)
                }
            }
        }
        val b = Array(q.size) { IntArray(2) { 0 } }
        var j = 0
        while (q.isNotEmpty()) {
            val n = q.pop()
            b[j][0] = a[n][1]
            b[j][1] = a[n][3]
            j++
        }
        b.sortWith(compareBy { it[1] })
        val res = mutableListOf<Int>()
        for (element in b) {
            res.add(element[0])
        }
        return res
    }
}
```