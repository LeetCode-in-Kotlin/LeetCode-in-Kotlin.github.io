[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1776\. Car Fleet II

Hard

There are `n` cars traveling at different speeds in the same direction along a one-lane road. You are given an array `cars` of length `n`, where <code>cars[i] = [position<sub>i</sub>, speed<sub>i</sub>]</code> represents:

*   <code>position<sub>i</sub></code> is the distance between the <code>i<sup>th</sup></code> car and the beginning of the road in meters. It is guaranteed that <code>position<sub>i</sub> < position<sub>i+1</sub></code>.
*   <code>speed<sub>i</sub></code> is the initial speed of the <code>i<sup>th</sup></code> car in meters per second.

For simplicity, cars can be considered as points moving along the number line. Two cars collide when they occupy the same position. Once a car collides with another car, they unite and form a single car fleet. The cars in the formed fleet will have the same position and the same speed, which is the initial speed of the **slowest** car in the fleet.

Return an array `answer`, where `answer[i]` is the time, in seconds, at which the <code>i<sup>th</sup></code> car collides with the next car, or `-1` if the car does not collide with the next car. Answers within <code>10<sup>-5</sup></code> of the actual answers are accepted.

**Example 1:**

**Input:** cars = \[\[1,2],[2,1],[4,3],[7,2]]

**Output:** [1.00000,-1.00000,3.00000,-1.00000]

**Explanation:** After exactly one second, the first car will collide with the second car, and form a car fleet with speed 1 m/s. After exactly 3 seconds, the third car will collide with the fourth car, and form a car fleet with speed 2 m/s.

**Example 2:**

**Input:** cars = \[\[3,4],[5,4],[6,3],[9,1]]

**Output:** [2.00000,1.00000,1.50000,-1.00000]

**Constraints:**

*   <code>1 <= cars.length <= 10<sup>5</sup></code>
*   <code>1 <= position<sub>i</sub>, speed<sub>i</sub> <= 10<sup>6</sup></code>
*   <code>position<sub>i</sub> < position<sub>i+1</sub></code>

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class Solution {
    fun getCollisionTimes(cars: Array<IntArray>): DoubleArray {
        val stack: Deque<Int> = LinkedList()
        val n = cars.size
        val ans = DoubleArray(n)
        for (i in n - 1 downTo 0) {
            ans[i] = -1.0
            val presentCar = cars[i]
            val presentCarSpeed = presentCar[1]
            while (stack.isNotEmpty()) {
                val previousCar = stack.peekLast()
                val previousCarSpeed = cars[previousCar][1]
                if (presentCarSpeed > previousCarSpeed &&
                    (
                        ans[previousCar] == -1.0 ||
                            catchTime(cars, i, previousCar) <= ans[previousCar]
                        )
                ) {
                    ans[i] = catchTime(cars, i, previousCar)
                    break
                }
                stack.pollLast()
            }
            stack.offerLast(i)
        }
        return ans
    }

    private fun catchTime(cars: Array<IntArray>, presentCar: Int, previousCar: Int): Double {
        val dist = cars[previousCar][0] - cars[presentCar][0]
        val speed = cars[presentCar][1] - cars[previousCar][1]
        return 1.0 * dist / speed
    }
}
```